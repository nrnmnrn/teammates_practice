# SP-03：Candidate Evaluation Loop

## 交付結果

交付「全部既有 Skills 失敗後，安全評估新策略」的完整路徑：Planner 提出唯一版本的 Candidate，Evaluator 在隔離 sandbox 套用 gate，Critic 將可讀失敗原因回饋 Planner，最多五版；失敗、契約違反與外部錯誤均留下可見證據，且從不修改 Live Run。

預估比賽當日拆為 2–3 張 vertical-slice Ticket。Owner、parent issue 與 Ticket 在比賽當日決定。

## 範圍

- 消費 all-skills-failed context 後才呼叫 provider-neutral Planner，產生唯一 `candidate_id`、父版本、原因、policy code 與 workload scope。
- 在 sandbox 使用與 baseline 相同 workload、seed、初始 state、evaluation window 執行 Candidate；驗證 Job、deadline、單 worker、不搶占、event 順序、有效 Snapshot 與可序列化 metrics。
- 實作 expired／completed／P95 evaluator gate、退化說明、Critic feedback、最多五版迭代及五版失敗結束。
- 在 adaptation UI 插入點顯示 Candidate 版本、sandbox 結果、gate、feedback 與停止原因；清楚區分 sandbox 與 Live Run。
- 外部 Planner、Evaluator、Critic 或 LLM timeout、schema、服務錯誤時停止目前 loop、保留最後有效 Snapshot 並顯示錯誤。

## 不在範圍

不將 Candidate 寫入 Skill Library，不啟用 Candidate，不改變 live Policy 或 segment；這些由 SP-04 負責。不得以 Mock 取代失效外部服務，除非明確啟用離線 Mock mode 並標示來源。

## 介面與直接依賴

依賴 SP-02 的 TriggerResult、既有 Skill EvaluationResult、all-skills-failed 判定與 adaptation context；依賴 SP-01／後端契約的 sandbox、Snapshot、metrics、events 與 UI 插入點。原因：Candidate 只能在既有 Skills 已被同條件否決後，才能安全比較。

開發時兩者為 **Contract** 依賴：可先用 deterministic all-failed context、baseline run 與 fake Planner／Evaluator 平行開發。正式聯合驗收另為 **Integration** 依賴，須接上 SP-01、SP-02 的契約實作。向 SP-04 提供 gate-passed Candidate、完整 EvaluationResult 與不可變 promotion input。

## 必要行為

- 全部既有 Skills 失敗前不得建立 Candidate；有任一通過時本 loop 不得執行。
- 每版 Candidate ID 唯一，並保存 parent、原因、code、workload、evaluation、feedback 與失敗理由。
- Gate 條件：expired 下降；或相同時 completed 增加且 P95 不惡化；completed 不得下降；P95 不得惡化；全部契約檢查與 sandbox 執行成功。
- 不通過、執行錯誤或契約違反時回饋 Planner；第五版仍失敗時停止並建立 `adaptation_failed` evidence，維持 Live Policy。
- sandbox 的 Job、events、metrics、Skills 與 Candidate 變化不得回寫到 Live Run。
- UI 明確標示資料來源與 Mock 狀態；sandbox metrics 不得作為 live 改善宣稱。

## 驗收與證據

- 對 all-skills-failed 前、單版拒絕、Critic 後續版本、第五版失敗、gate 通過、契約違反與 adapter timeout 寫 deterministic 測試。
- 每項評估可對照相同 baseline inputs，並證明 Live Run Snapshot 在 loop 前後不變。
- 驗證 Candidate ID／parent chain、最多五版、feedback、gate 原因、`adaptation_failed` 與 UI 證據完整。
- 驗證 Demo mode 不顯示人工 accept，外部錯誤不 fallback Mock，最後有效 Snapshot 可讀。

## 尚待決定

外部 adapter 的錯誤是否必須另寫特定領域 Event，以及 UI 重試按鈕的種類、時機與自動／手動行為，尚待決定。此子 PRD 已確認的最低行為是停止 loop、保留最後有效 Snapshot、顯示錯誤；不得自行擴張為未核定的重試流程。
