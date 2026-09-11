# Agent Orchestration 子 PRD

> 父規格：[`docs/PRD.md`](../PRD.md)。本文件是 Agent adaptation 的可執行子 PRD，不再繼續拆分。Scheduler、UI 與 Sandbox 的細節分別由既有契約及其他模組文件負責。

## 1. 目標與邊界

當 workload 改變並造成指標惡化時，Agent 必須先嘗試重用 Skill Library 內已驗證的 Skills；只有全部既有 Skills 都未通過 evaluator，才建立 Candidate。通過 evaluator 的 Skill 由 Agent 自動註冊、啟用，並在下一次 dispatch 生效。

本子 PRD 負責 adaptation 的狀態、決策、版本迭代、事件與錯誤恢復。Live Run 的排程執行、sandbox 的隔離與 Gradio 的呈現由其他模組提供 adapter。

## 2. 名詞

| 名詞 | 定義 |
|---|---|
| Policy | worker 選擇下一筆 job 的規則。 |
| Skill | 已通過 evaluator、可直接啟用的 policy。 |
| Candidate | 尚未通過 evaluator 的 policy 版本。 |
| Planner | 根據 workload、metrics 與 feedback 產生或修改 Candidate。 |
| Evaluator | 在 sandbox 比較 baseline 與 Skill／Candidate。 |
| Critic | 分析失敗結果並產生給 Planner 的修正 feedback。 |
| Adaptation | 從 trigger 到 Skill 重用或 Candidate 啟用的一次流程。 |

初始 Skills 固定為 FIFO、SJF、Priority、EDF。Hybrid 只有通過 evaluator 後才能成為 Skill；Reset 移除後續版本。

## 3. 狀態機

```text
idle
→ triggered
→ evaluating_existing
→ existing_skill_selected
→ (或 proposing_candidate → evaluating_candidate → critic_feedback → proposing_candidate)
→ registering
→ activating
→ completed
```

錯誤狀態為 `failed`。外部 adapter timeout、執行錯誤或不可解析回應時停止目前 adaptation，保留最後有效 Snapshot，不自動改用 Mock。

同一 `run_id` 與同一 workload window 只允許一個進行中的 adaptation。重複 trigger 必須合併或忽略，不能並行啟用兩個 policy。

## 4. 既有 Skill 優先流程

1. `detect_adaptation_trigger(snapshot)` 判斷是否達到 trigger。
2. 以相同 workload、seed、初始 simulator state、evaluation window 與 baseline 評估所有已驗證 Skills。
3. 若目前 policy 通過，維持目前 policy，不建立 Candidate。
4. 若其他 Skill 通過，依下列順序選最佳者：expired count ASC、completed count DESC、P95 latency ASC、目前 policy match DESC、`skill_id` ASC。
5. 記錄 `existing_skill_reused` 與 `policy_activated` 事件；running job 保留，下一次 dispatch 使用選定 Skill。

既有 Skill 通過時，禁止呼叫 Planner、建立 Candidate 或增加 Skill Library 項目。

## 5. Candidate 迭代流程

只有 adaptation trigger 已成立、所有既有 Skills 失敗、失敗原因已記錄且尚未達 5 版時，才可呼叫 Planner。

Candidate ID 使用不可重複版本，例如 `hybrid-v1` 至 `hybrid-v5`。每版記錄 `candidate_id`、`parent_candidate_id`、產生原因、policy code、適用 workload、Evaluator 結果與 Critic feedback。

- 執行錯誤或契約違反：記錄 Critic feedback，產生下一版。
- 指標未通過 gate：記錄退化指標與 feedback，產生下一版。
- 通過 gate：自動註冊 Skill、建立啟用事件，下一次 dispatch 生效。
- 5 版全部失敗：維持原 policy，建立 `adaptation_failed` 事件。

## 6. Evaluator gate

Evaluator 必須驗證：expired count 優先改善；completed count 不下降；P95 latency 不惡化；Job／deadline／單 worker／不搶占／事件順序契約正確；sandbox 可執行並產生完整可序列化 Snapshot。

結果至少包含 baseline metrics、candidate metrics、expired／completed／P95 比較、contract validation、sandbox execution status、passed／failed、failure reason 與 Critic feedback。

## 7. Automatic activation

啟用流程建立 `policy_activated` 事件、更新 current policy、建立新的 policy segment，保留目前 running job，禁止搶占或重新派工，並將新 policy 設為下一次 dispatch 的規則。Live Run 的混合 metrics、segment metrics 與 sandbox 結果分開保存。

## 8. 模式與錯誤

Demo mode 預設由 Agent 自動決策，User 只能控制播放、重設與 workload；不得手動切換 policy 或接受 Candidate。Developer mode 才能手動套用已驗證 Skill。

外部 Planner／Evaluator／Critic 發生 timeout、schema 錯誤或服務錯誤時，狀態轉為 `failed`，停止 adaptation、保留最後有效 Snapshot、顯示可重試訊息並保留錯誤事件。只有明確指定的離線 Mock mode 可使用 Mock，且 UI 必須標示資料來源。

## 9. 介面與事件

```text
detect_adaptation_trigger(snapshot)
evaluate_existing_skills(context)
select_existing_skill(results)
propose_candidate(context, feedback)
evaluate_candidate(candidate_id, baseline_run)
register_skill(candidate_id)
activate_skill(skill_id)
```

所有呼叫與事件關聯 `run_id`、`snapshot_version`、`adaptation_id`、`candidate_id`、`parent_candidate_id`、`policy_id` 與事件序號。介面必須明訂 timeout、錯誤狀態與 sandbox／live 隔離。

## 10. 驗收條件

- Trigger 未成立時不啟動 adaptation。
- 目前或其他既有 Skill 通過時不呼叫 Planner，且依固定順序選擇並自動啟用。
- 所有既有 Skills 失敗後才產生 Candidate，最多 5 版。
- Candidate 通過後自動註冊並於下一次 dispatch 生效；running job 不被中斷。
- Sandbox 不修改 Live Run，四類 metrics 可分開查詢。
- Demo mode 隱藏手動切換；Developer mode 可測試手動切換。
- timeout／錯誤停止 adaptation 並保留最後有效 Snapshot。
- 相同輸入與版本資訊可重現相同選擇與事件序列。

## 11. 依賴與未完成項

依賴 `docs/PRD.md`、`docs/backend-contract.md`、Scheduler Backend adapter、Sandbox Evaluation adapter、Skill Library adapter 與 UI controller。外部 LLM／Planner／Evaluator／Critic 的實際供應商與部署方式不在本子 PRD 決定；正式 Demo 必須由 provider-neutral adapter 接通。
