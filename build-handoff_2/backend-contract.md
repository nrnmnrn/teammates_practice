# UI ↔ Backend contract

> 本文件是 [`PRD.md`](PRD.md) 的串接契約文件，不是子 PRD。Agent orchestration 的可執行規格見 [`prd/AGENT_ORCHESTRATION_PRD.md`](prd/AGENT_ORCHESTRATION_PRD.md)；本文保留 UI／Simulator 欄位細節，並以總 PRD 及本文末更新契約為準。

本版使用同程序 `scheduler.Simulator`。UI 的 session controller 管理播放、倍速與錯誤呈現；simulator 是排程時間、狀態及指標的唯一來源。

## 方法

| 方法 | 行為 |
| --- | --- |
| `Simulator(seed=42, initial_jobs=None)` | 預設建立 8 筆可重現的訂單；`initial_jobs=[]` 可建立空場景供測試。 |
| `reset(seed=42)` | 建立新 run，回復時間 0、FIFO、初始四個 skills 與 8 筆訂單。 |
| `advance(dt)` | 推進非負模擬時間，依序处理區間內所有事件。 |
| `inject(jobs)` | 原子加入 `Job` 清單；到達時間不可早於現在，累計不得超過 20 筆。 |
| `generate(count)` | 使用場景自己的隨機產生器，在現在時間注入指定筆數。 |
| `set_policy(id, reason=...)` | 改變下次派工的策略，不打斷目前 request。 |
| `snapshot()` | 回傳與內部狀態分離、可序列化的快照。 |
| `list_skills()` | 回傳目前可用策略與 metadata。 |
| `propose_candidate(context, feedback)` | 在所有既有 Skills 失敗後建立有版本的 Candidate。 |
| `evaluate_candidate(candidate_id, baseline_run)` | 在 sandbox 執行 Candidate 並回傳 evaluator gate 與 feedback。 |
| `register_skill(candidate_id)` | 僅在 evaluator 通過後自動登錄 Candidate。 |
| `activate_skill(skill_id)` | 建立啟用事件，保留 running job，下一次 dispatch 生效。 |

`Job(id, arrival, processing_time, priority, deadline)` 的時間單位為模擬秒。ID 必須非空且唯一，數值必須有限，工時須大於零，deadline 須晚於 arrival。無效批次整批拒絕，不留下部分新增資料。

策略 ID：`fifo`、`sjf`、`priority`、`edf`；Hybrid 及其版本只有通過 evaluator 後才可套用。原始碼由實際執行的 Python key function 取得，不對展示字串使用 `eval` 或 `exec`。

## 快照

| 欄位 | 內容 |
| --- | --- |
| `run_id`, `time` | 本次場景識別與目前模擬秒數。 |
| `jobs` | 工作輸入欄位，以及 `status`、`started_at`、`completed_at`、`dropped_at`、`feasible`。 |
| `worker` | `job_id`、`state`、`reason`。 |
| `policy` | `id`、`name`、`code`、`previous_code`、`reason`。 |
| `metrics` | `completed`、`expired`、`throughput`、`p95_latency`。未定義值為 `None`。 |
| `events` | 含 `seq`、`time`、`type`、`job_id`、`policy_id`、`message` 的有序事件。 |
| `series` | 語意事件時間點的 metrics 歷史，各列包含 `time`。 |
| `adaptation` | `stage`、`message`、`candidate_id`。 |
| `capacity` | `total`、`limit`、`remaining`。 |

工作狀態為 `scheduled`、`pending`、`running`、`completed`、`expired`。最後兩者為終態。候選階段為 `idle`、`proposed`、`evaluating`、`registered`、`failed`。

Skill record 包含 `id`、`name`、`description`、`code`、`source`、`uses`、`last_applied_at`。來源必須區分初始策略、外部 evaluator 驗證的 Candidate 與明確離線 Mock。

## 時間與狀態一致性

## 自動 adaptation（Demo mode）

Demo mode 不提供使用者 policy switch 或 Candidate accept。當 workload 達到 adaptation trigger 時，adapter 必須先以相同 seed、workload、初始狀態與 evaluation window 評估所有現有且已驗證的 skills。

- 若目前 policy 已能通過 gate，維持目前 policy，不建立 Candidate。
- 若其他既有 skill 通過，依 expired、completed、P95、目前 policy、skill ID 的固定順序選出最佳者，記錄 `existing_skill_reused`，並在下一次 dispatch 自動啟用。
- 只有所有既有 skills 都失敗，Planner 才能建立 Candidate。
- Candidate 最多嘗試 5 個版本；失敗原因與 Critic feedback 必須保留。通過者自動登錄 Skill Library 並在下一次 dispatch 啟用；全部失敗則維持原 policy。
- Sandbox 只能修改複本，不得修改 live Run。正在處理的 job 不會被中斷。

必要的 adapter 能力如下：

```text
detect_adaptation_trigger(snapshot)
evaluate_existing_skills(context)
select_existing_skill(results)
propose_candidate(context, feedback)
evaluate_candidate(candidate_id, baseline_run)
register_skill(candidate_id)
activate_skill(skill_id)
```

Developer mode 可用 `uv run python app.py --mode developer` 啟用手動切換與舊的 Mock candidate controls；該模式只供測試，不是正式 demo 流程。

同時刻事件順序：完成 → 丟棄逾時工作 → 納入到達工作 → 派工。派工先做 `time + processing_time <= deadline` 篩選；不合格工作繼續等待至 deadline。

初始場景在時間 0 處理到達與派工；暫停只凍結時間。更新間隔不得改變事件順序、完成時間或指標。趨勢取樣使用事件時間；瀏覽器補動畫不回寫狀態，也不自行判斷工作已完成。

每個瀏覽器 session 使用自己的 controller 與 simulator。UI 依序處理同 session 的操作，快照帶 run 與更新版本識別；重設後不接受舊畫面更新。不同 session 互不共用工作清單。

輸入驗證失敗或後端錯誤應向 controller 回報例外；controller 停止播放，保留最後有效快照並顯示錯誤與可重試操作。接入真實後端時仍需維持這些行為。

## Agent adaptation 更新契約

正式 Demo 不使用 `resolve_candidate(accept)` 作為必要流程。若保留此方法，只能作為 Developer mode 的相容測試入口；Candidate 是否成為 Skill 由 Evaluator gate 決定，通過後自動註冊與啟用。

Provider-neutral adapter 必須提供：

```text
detect_adaptation_trigger(snapshot)
evaluate_existing_skills(context)
select_existing_skill(results)
propose_candidate(context, feedback)
evaluate_candidate(candidate_id, baseline_run)
register_skill(candidate_id)
activate_skill(skill_id)
```

Adaptation 順序固定為：先評估 Skill Library 內所有已驗證 Skills；有通過者依 expired、completed、P95、目前 policy 是否相同、`skill_id` 的順序選最佳者並記錄 `existing_skill_reused`。只有全部失敗才呼叫 Planner。Candidate 最多 5 版；每版的 candidate ID、父版本、失敗原因、Critic feedback 與 evaluator 結果都要可追蹤。通過後建立註冊與啟用事件，保留 running job，下一次 dispatch 才使用新 policy；5 版全失敗則維持原 policy。

Evaluator 必須在與 baseline 相同的 workload、seed、初始狀態與 evaluation window 下執行。Sandbox 不得修改 live Run。結果至少包含 baseline metrics、candidate metrics、gate、錯誤與 feedback，並與 live metrics、policy segment metrics 分開保存。

正式 Demo 的外部 Planner／Evaluator／Critic 或 LLM adapter 發生 timeout／錯誤時，停止 adaptation、保留最後有效 Snapshot 並顯示錯誤；不得默默改用 Mock。離線 Mock 必須由明確模式啟用並顯示資料來源。
