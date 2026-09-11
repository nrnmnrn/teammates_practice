# UI、Scheduler 與 Agent Adapter 契約

本契約定義 implementation repository 中 UI、scheduler、sandbox 與 Agent adapter 的共同邊界。若與 [總 PRD](../../PRD.md) 衝突，以總 PRD 為準；若子 PRD 對自己的直接依賴提出更嚴格要求，以子 PRD 為準。本文件不決定 LLM 供應商、部署方式或實作類別。

## 共同規則

- 同程序 Python factory：`create_backend(seed: int = 42, initial_jobs: list[dict] | None = None) -> BackendAdapter`。每個瀏覽器 session 呼叫一次；不得回傳共享的可變 singleton。
- `initial_jobs=None` 建立預設八筆 Job；`[]` 建立空場景。factory 完成 `t=0` 的到達與 dispatch 處理後才回傳。
- 公開 API 的 number 只接受內建 `int` 或 `float`，排除 `bool`、字串與 Decimal；輸出僅含 JSON 可序列化、有限的數值。內部可採 Decimal 保護邊界。
- 每個成功的修改操作回傳新的 Snapshot。Snapshot 是防禦性副本；讀取不可改變狀態。
- UI controller 對同一 session 的操作序列化；三個 tabs 不可各自推進或讀取不同狀態。

## BackendAdapter 方法

| 方法 | 回傳 | 契約 |
| --- | --- | --- |
| `reset(seed=42)` | Snapshot | 新 run；時間 0、FIFO、八筆初始 Job、四個初始 Skills；清除 events、series、後續 Skills、Candidate 與 usage。 |
| `advance(dt)` | Snapshot | `dt` 為有限非負數；處理區間內全部事件；零不推進時間。 |
| `inject(jobs)` | Snapshot | 驗證整批後原子加入；arrival 不早於現在；累計不超過 20。 |
| `generate(count)` | Snapshot | 僅接受整數 1 或 4；使用本 run 的 RNG 在現在時間產生並 inject。 |
| `set_policy(id, reason)` | Snapshot | 僅 developer mode；Skill 必須已驗證；不搶占；同 Policy 無狀態變更。 |
| `snapshot()` | Snapshot | 無副作用。 |
| `list_skills()` | `list[Skill]` | 回傳本 run 所有可用 Skills 與使用記錄；讀取不改變狀態。 |
| `detect_adaptation_trigger(snapshot)` | TriggerResult | 回傳是否觸發及可見原因。 |
| `evaluate_existing_skills(context)` | `list[EvaluationResult]` | 用相同 baseline 條件評估每個已驗證 Skill。 |
| `select_existing_skill(results)` | Skill 或 null | 僅在至少一個通過時依總 PRD 的固定順序選擇。 |
| `propose_candidate(context, feedback)` | Candidate | 僅在既有 Skills 全部失敗且未達五版時建立。 |
| `evaluate_candidate(candidate_id, baseline_run)` | EvaluationResult | 僅在 sandbox 執行；不得修改 live Run。 |
| `register_skill(candidate_id)` | Skill | 僅接受通過 gate 的 Candidate。 |
| `activate_skill(skill_id)` | Snapshot | 建立新 segment 與 `policy_activated`；下一次 dispatch 才使用。 |

team mode 的 factory 載入失敗必須明確失敗，不得 fallback 到 local。local 或 Mock 僅能以明確 mode 選擇，且 UI 必須標示來源。

## 排程資料

### JobInput 與 Job

JobInput 為 `id`、`arrival`、`processing_time`、`priority`、`deadline`。ID 非空且同 run 唯一；arrival 非負；processing time 正；deadline 大於 arrival；所有數值有限。無效整批必須拒絕，不消耗 RNG，不留部分資料。

Job Snapshot 另含：`status`、`started_at`、`completed_at`、`dropped_at`、`feasible`、`dispatched_policy_id`、`segment_id`。`feasible` 僅對 pending／running 有值；終態與 scheduled 為 null。後兩個欄位由 dispatch 寫入，支援已確認的 Policy／segment 證據。

狀態與同時刻事件順序、Policy 排序、deadline 邊界、單 worker、不搶占，均依總 PRD。事件序號 `seq` 在同一 run 遞增。

### Skill、Candidate、EvaluationResult

Skill 至少有 `id`、`name`、`description`、`code`、`source`、`verified`、`uses`、`last_applied_at`。`source` 為 `base`、`candidate` 或明確的 `mock`。`uses` 只計實際 dispatch，不計預覽或按鈕點擊。

Candidate 至少有 `candidate_id`、`parent_candidate_id`、`version`、`reason`、`policy_code`、`workload_scope`、`status`、`critic_feedback`、`evaluation_result`。狀態可為 `proposed`、`evaluating`、`failed`、`passed`、`registered`；ID 不可重複。

EvaluationResult 至少有 `subject_id`、`subject_kind`、`baseline_metrics`、`evaluated_metrics`、`evaluation_window`、`gate_passed`、`regressions`、`contract_validation`、`sandbox_status`、`failure_reason`、`critic_feedback`。它必須明示 sandbox 結果，不能混入 Live Run metrics。

尚待決定：除上述最小欄位外，Skill 是否需要其他 metadata，以及其資料保存範圍。實作者不得自行把未核定 metadata 變成 UI 或 gate 的必要條件。

## Snapshot 與 Event

Snapshot 至少包含：

```text
run_id, snapshot_version, time, jobs, worker, policy, metrics,
segments, events, series, adaptation, capacity
```

- `run_id`：每次 reset 改變。
- `snapshot_version`：同一 run 單調遞增；供 UI 拒收過時回應。
- `worker`：`job_id`、`state`（`idle`／`running`）、`reason`。
- `policy`：`id`、`name`、`code`、`previous_code`、`reason`。
- `metrics`：`completed`、`expired`、`throughput`、`p95_latency`；無定義值用 null。
- `segments`：已啟用 Policy 的時間範圍與 segment metrics；不得與整局或 sandbox 指標混淆。
- `series`：按事件時間排序的整局 metrics 歷史；同時刻可合併。
- `adaptation`：`stage`、`message`、`adaptation_id`、`candidate_id`。
- `capacity`：`total`、固定 `limit=20`、`remaining`。

Event 至少有 `seq`、`time`、`type`、`job_id`、`policy_id`、`candidate_id`、`message`。必要類型為排程事件 `arrived`、`started`、`completed`、`expired`，以及 `existing_skill_reused`、`candidate_proposed`、`candidate_evaluated`、`skill_registered`、`policy_activated`、`adaptation_failed`。無關 ID 用 null。

`policy_activated`、Job 的 Policy／segment 記錄與 segment metrics 是既定產品需求。是否另為每種 adapter 失敗建立領域 Event，以及錯誤後提供何種重試提示與時機，尚待決定；目前已確認的行為只有停止 adaptation、保留最後有效 Snapshot 並顯示錯誤。

## Metrics

- Live Run：整局 `completed`、`expired`、`throughput`、P95 completion latency。
- Policy segment：每個已啟用 Policy 的區段結果。
- Existing Skill evaluation：相同 baseline 下各既有 Skill 的 sandbox 結果。
- Candidate evaluation：各 Candidate 版本的 sandbox 結果。

throughput 為 `completed / (time / 60)`；時間為零時為 null。P95 只用 completed Jobs 的 `completed_at - arrival`，空集合為 null，採線性插值。卡片可四捨五入顯示，測試比較原始值。

## Session 與錯誤

UI envelope 另持有 `session_generation`、`revision`、`backend_source`、`playing`、`speed`、`error`。reset 增加 generation 並換 run ID；過時 generation、run ID 或 revision 的結果不得套用到任何 tab。

輸入錯誤以 `AdapterValidationError(ValueError)` 回報，並保證 Job、RNG、events、時間不變。操作或讀取失敗以 `AdapterOperationError(RuntimeError)` 回報，含 `state_uncertain: bool`；無法證明未修改時預設為 true。controller 必須暫停、保留最後有效 Snapshot、顯示錯誤並提供重新取得狀態或 reset。狀態不明前不得自動重送修改操作。不得把 stack trace 當使用者文案。

外部 Planner、Evaluator、Critic 或 LLM timeout／schema／服務錯誤時，adaptation 進入失敗狀態。不得默默改用 Mock。
