# Adaptive Scheduler Arena：總 PRD

## 文件定位

本文件是產品需求的唯一權威。它與 [後端契約](docs/contracts/backend-contract.md) 及四份子 PRD 一起交付給未來 implementation repository。`build-handoff_final` 尚待獨立驗收；在驗收通過前，它是候選 handoff，不宣稱已取代其他文件。

本產品為一個可觀察、可驗證的排程模擬展示：workload 改變造成服務指標惡化時，Agent 先重用已驗證 Skill；全部失敗後才產生、隔離評估及晉升新的 Candidate。正式展示中，使用者只控制模擬與 workload，不能代替 Agent 作 policy 決策。

## 成功結果

- 評審可看見 workload 改變、指標惡化、Agent 決策原因與 policy 生效時點。
- 有既有 Skill 通過 evaluator 時，系統重用最佳者，不建立 Candidate。
- 全部既有 Skills 失敗時，Planner 與 Critic 最多產生五個 Candidate 版本。
- Candidate 通過 evaluator 後自動登錄為 Skill，並在下一次 dispatch 啟用。
- Live Run、policy segment、既有 Skill evaluation 與 Candidate evaluation 的 metrics 明確分開。
- 三個 Gradio tabs、動畫、Skill Library、metrics 與 event log 顯示同一份 Snapshot。
- 90 秒 demo 可重複展示正常 workload、Flash Sale、adaptation 與自動啟用。

## 名詞與產品邊界

| 名詞 | 定義 |
| --- | --- |
| Job | 一筆可被排程的訂單。 |
| Policy | worker 選擇下一筆 Job 的排序規則。 |
| Skill | 已通過 evaluator、可在 live Run 使用的 Policy。 |
| Candidate | 尚未通過 evaluator 的新 Policy 版本。 |
| Live Run | 正式展示的模擬狀態。 |
| Sandbox Run | 評估專用複本；不得修改 Live Run。 |
| Policy segment | 同一 Policy 連續生效的區段。 |

初始 Skills 僅有 FIFO、SJF、Priority、EDF。Hybrid 的排序函式可存在於程式中，但 Hybrid 不屬於初始 Skill，必須通過 evaluator 才能登錄。reset 必須移除後續新增 Skill，恢復四個初始 Skills。

本版不包含 Round Robin、搶占、worker failure、多 worker、長期 evaluator、登入或部署。不得執行未經 sandbox 驗證的動態 policy code。

## 排程與模擬規則

- 全部時間為模擬秒。Job ID 非空且同一 run 唯一；數值有限；工時大於零；`deadline > arrival`。
- Job 狀態為 `scheduled`、`pending`、`running`、`completed`、`expired`；後兩者為終態。
- 單一 worker 一次只處理一筆 Job，開始後不搶占。切換 Policy 不打斷 running Job，新 Policy 從下一次 dispatch 生效。
- worker 空閒時，只在已到達的 pending Jobs 中保留可準時完成者：`now + processing_time <= deadline`，再依 Policy 排序。不可行 Job 保留至 deadline 才 expired。
- FIFO 依 `arrival, id`；SJF 依 `processing_time, arrival, id`；Priority 依 `priority` 由大至小、`arrival, id`；EDF 依 `deadline, arrival, id`；通過驗證的 Hybrid 依 `priority` 由大至小、`deadline, processing_time, arrival, id`。
- 同一模擬時刻依序處理：完成、丟棄逾期等待 Job、納入到達 Job、dispatch。恰在 deadline 完成算成功。
- `advance(dt)` 必須處理區間內所有事件；不同切分方式不可改變選擇、完成時間、event 順序或 metrics。
- 預設 seed 為 42，初始八筆可重現 Jobs。每個 session 有自己的 simulator 與 RNG。一次 inject 必須原子驗證；失敗不得改變 Job、event 或 RNG。run 總 Job 上限為 20，終態不釋放名額。

## 自動 adaptation

固定流程如下：

```text
workload 改變 → trigger → 評估既有 Skills
  ├─ 有通過者：選最佳 Skill → 自動啟用 → 下次 dispatch 使用
  └─ 全部失敗：Planner 提出 Candidate → sandbox 評估
       ├─ 失敗：Critic feedback → 下一版，最多五版
       └─ 通過：自動登錄 Skill → 自動啟用 → 下次 dispatch 使用
```

每個既有 Skill 必須在相同 workload、random seed、初始 simulator state、evaluation window 與 baseline Policy 下評估。若目前 Policy 通過，維持它；否則依下列穩定順序選擇通過的 Skill：expired count 最低、completed count 最高、P95 completion latency 最低、目前 Policy 相同者優先、`skill_id` 字典序。重用時記錄 `existing_skill_reused`；不得呼叫 Planner、建立 Candidate 或增加 Skill Library 項目。

Candidate 只能在所有既有 Skills 失敗、失敗原因已保留且尚未達五版時建立。每版使用唯一 ID，並保留 `candidate_id`、父版本、產生原因、policy code、適用 workload、evaluator 結果與 Critic feedback。執行錯誤、契約違反或 metrics 未改善都回饋 Planner。五版均失敗時維持原 Policy，記錄失敗結果，不建立 Skill。

Evaluator 對既有 Skill 與 Candidate 使用同一 gate：expired count 必須下降；或 expired 相同時 completed count 增加且 P95 不惡化；completed 不得下降；P95 不得惡化；並須通過 Job、deadline、單 worker、不搶占、event 順序、sandbox 執行、有效 Snapshot 與可序列化 metrics 檢查。結果必須保留 baseline、受測結果、退化項目、gate、失敗原因與 Critic feedback。

Candidate 通過後才可註冊。啟用建立 `policy_activated` event 與新的 policy segment，保留 running Job，不重新 dispatch。Job 在 dispatch 時記錄適用 Policy 與 segment。

尚待決定：同一 run、同一 workload window 的重複 trigger 要合併、忽略或排隊；不得讓未決策略同時啟用。segment metrics 對跨越切換時點的 Job 採何種歸屬計算，亦尚待決定；不得把未定義歸屬偽裝成可比較結果。

## Metrics、Snapshot 與 Adapter

Live Run metrics 是整局混合 Policy 的 completed、expired、throughput 與 P95。Policy segment metrics 是各 Policy 生效區段的結果。既有 Skill evaluation 與 Candidate evaluation 都是 sandbox 結果。sandbox 結果不得冒充 live Run 改善證據。

系統由可替換、provider-neutral 的 adapter 組成：Gradio UI、session/integration controller、SchedulerBackendAdapter、PlannerAdapter、EvaluatorAdapter、CriticAdapter。所有 adapter 的輸入、輸出、timeout、錯誤、run ID、Snapshot version、Candidate version 與 sandbox/live 隔離，以 [後端契約](docs/contracts/backend-contract.md) 為準。

最低能力：

```text
detect_adaptation_trigger(snapshot)
evaluate_existing_skills(context)
select_existing_skill(results)
propose_candidate(context, feedback)
evaluate_candidate(candidate_id, baseline_run)
register_skill(candidate_id)
activate_skill(skill_id)
```

## Demo 與 Developer mode

Demo mode 為預設。使用者可播放、暫停、單步、reset、調整 0.5×／1×／2× 倍速，以及 inject workload；不得手動切換 Policy 或接受 Candidate。畫面必須顯示資料來源、evaluator 是否為 Mock、指標惡化原因、既有 Skill 比較、Candidate 與 Critic feedback、gate 結果、Skill 註冊、Policy 自動切換原因與生效時間。

Developer mode 僅供測試，可手動套用已驗證 Skill、測試 policy switching、segment metrics 與 Skill Library adapter；不屬正式展示。外部 Planner、Evaluator、Critic 或 LLM adapter timeout／錯誤時，停止 adaptation、保留最後有效 Snapshot 並顯示錯誤。不得無提示地切換 Mock；離線 Mock 必須明確指定並標示資料來源。

三個 tabs 為：

- **Scheduling Arena**：job 狀態、worker、控制、workload 與 SVG 動畫。
- **Metrics & Code**：同一 Snapshot 的 cards、趨勢、實際排序 code 與最近 Policy diff。
- **Skill Library**：Skill 規則、來源、code、使用記錄與已驗證 Skill 的 developer-mode 套用測試。

瀏覽器以 1440×900 驗收，並在 1280×720 確認控制項可換行且不重疊、無整頁水平溢出。畫面僅用 Snapshot 補動畫，不能自行判定 Job 完成或改寫狀態。

## 90 秒展示

| 秒數 | 必須看見的證據 |
| --- | --- |
| 0–15 | 正常 workload、一個 worker、目前 Policy。 |
| 15–30 | Flash Sale、workload 變化與指標惡化。 |
| 30–45 | 既有 Skill evaluation；若通過，顯示重用。 |
| 45–65 | 若全數失敗，顯示 Planner、Candidate 版本與 Critic feedback。 |
| 65–75 | evaluator 通過、Skill 自動註冊。 |
| 75–85 | 自動切換及下一次 dispatch 使用新 Skill。 |
| 85–90 | Skill Library、live 與 sandbox metrics 的差別。 |

不得把使用者操作說成 Agent 決策，也不得用單一 Run 的混合 metrics 宣稱新 Policy 對所有 workload 都較好。

## 子 PRD 責任

| 子 PRD | 完整功能責任 |
| --- | --- |
| [scheduling-arena](sub-PRDs/scheduling-arena.md) | 可操作、可觀察的基礎排程 Arena 與三 tabs 的基礎呈現。 |
| [existing-skill-adaptation](sub-PRDs/existing-skill-adaptation.md) | 偵測退化、比較及重用既有 Skill 的完整路徑。 |
| [candidate-evaluation-loop](sub-PRDs/candidate-evaluation-loop.md) | Candidate 的安全生成、sandbox 評估、Critic 迭代與失敗證據。 |
| [skill-promotion-and-recovery](sub-PRDs/skill-promotion-and-recovery.md) | 已通過 Candidate 的登錄、啟用、復原與可見證據。 |

每份子 PRD 預估 2–3 張 vertical-slice Ticket；4–5 張須重新檢查邊界、記錄不拆理由並取得特別核准；超過 5 張必須拆分或正式縮減範圍。Ticket、Owner 與 GitHub issue 僅在比賽當日依正式流程決定。
