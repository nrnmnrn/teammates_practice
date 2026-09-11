# SP-02：Existing Skill Adaptation

## 交付結果

交付「指標惡化後重用既有 Skill」的完整可見路徑：系統偵測 trigger，在相同 sandbox 條件比較所有已驗證 Skills，依固定規則選出最佳通過者，自動啟用，並在 UI 顯示原因、比較與 event；此路徑絕不建立 Candidate。

預估比賽當日拆為 2–3 張 vertical-slice Ticket。Owner、parent issue 與 Ticket 在比賽當日決定。

## 範圍

- 實作 `detect_adaptation_trigger`、既有 Skill evaluation、固定選擇順序與 `existing_skill_reused` 證據。
- 為每個已驗證 Skill 建立相同 workload、seed、初始 state、evaluation window 與 baseline 的 sandbox evaluation。
- 對目前 Policy 通過、其他 Skill 通過、沒有 Skill 通過三種結果，提供可觀察的 adaptation stage、原因與比較結果。
- 經既定 `activate_skill` seam 自動安排下次 dispatch 使用選定 Skill；不搶占 running Job。
- 在既有 UI shell 顯示 degradation 原因、每項既有 Skill 的 baseline／受測 metrics、選擇原因與啟用 event。

## 不在範圍

不產生 Candidate、不呼叫 Planner、不註冊 Skill、不定義 Candidate evaluator gate 或 Critic feedback。也不修改基礎 scheduler 的排程規則、UI shell 或 Snapshot 基本結構。

## 介面與直接依賴

依賴 SP-01 提供的 SchedulerBackendAdapter、Snapshot、已驗證 Skills、整局 metrics、Policy activation seam、events 及 adaptation UI 插入點。原因：此功能要以同一份可重現的 scheduling evidence 判斷與展示重用結果。

開發依賴類型為 **Contract**：後端契約凍結後可用 deterministic fixture／adapter 開發與測試。與 Arena 的聯合驗收另為 **Integration** 依賴，必須使用 SP-01 成果。向 SP-03 提供 `TriggerResult`、完整既有 Skill `EvaluationResult[]`、all-skills-failed 判定與可追溯 adaptation context。

## 必要行為

- trigger 未成立時不得開始 evaluation 或啟用 Policy。
- 評估範圍只含已驗證 Skills，且每項使用相同 baseline 條件。
- 若目前 Policy 通過，維持目前 Policy；若多個通過，依 expired 最低、completed 最高、P95 最低、目前 Policy、`skill_id` 選擇。
- 有既有 Skill 通過時，必須記錄 `existing_skill_reused` 與 `policy_activated`，但不得建立 Candidate、呼叫 Planner 或改變 Skill Library。
- 啟用只影響下一次 dispatch；running Job 保留。sandbox 不得修改 Live Run。
- Demo mode 只呈現自動結果；Developer mode 的手動 Skill 測試不得偽裝成這條路徑的 Agent 決策。

## 驗收與證據

- trigger 未成立、目前 Policy 通過、多個通過的 tie-break、所有失敗各有 deterministic 測試。
- 所有 sandbox evaluation 都證明 baseline 條件相同且不改 Live Run Snapshot。
- 有 Skill 通過時，測試證明無 Candidate、無 Planner 呼叫、無新 Skill；下次 dispatch 才採新 Policy。
- UI／event／metrics 用同一 Snapshot 顯示 degradation、比較、選擇與啟用原因。
- team 與 deterministic adapter／fixture 都可重跑上述結果；真實整合在 SP-01 完成後驗證。

## 尚待決定

同一 workload window 的重複 trigger 要合併、忽略或排隊尚待決定。實作者可防止同時啟用多個 Policy，但不得把特定合併或併行策略標為已核定產品行為。
