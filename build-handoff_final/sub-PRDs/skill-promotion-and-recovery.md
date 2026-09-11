# SP-04：Skill Promotion and Recovery

## 交付結果

交付「只有合格 Candidate 進入正式執行」的完整路徑：接收已通過 gate 的 Candidate，自動註冊為 Skill、顯示於 Skill Library、建立 policy segment 與 `policy_activated` event，於下一次 dispatch 啟用；註冊／啟用失敗時維持最後有效狀態，reset 回復初始 Skills。

預估比賽當日拆為 2–3 張 vertical-slice Ticket。Owner、parent issue 與 Ticket 在比賽當日決定。

## 範圍

- 驗證 promotion input 的 Candidate gate-passed EvaluationResult，再註冊成 `verified=true`、`source=candidate` 的 Skill。
- 自動啟用已註冊 Skill，建立新的 Policy segment 與 `policy_activated` evidence；running Job 不被中斷，新 Skill 從下一次 dispatch 生效。
- 在 Skill Library 與 adaptation UI 呈現註冊來源、code、使用記錄、gate 依據、啟用原因與生效時點。
- 實作註冊、啟用、後續 Snapshot 讀取失敗的錯誤處理：暫停、保留最後有效 Snapshot、顯示錯誤；狀態不明時先重新同步或 reset，不自動重送修改。
- reset 清除後續 Candidate Skills、segments、usage 與 adaptation 狀態，恢復 FIFO、SJF、Priority、EDF。

## 不在範圍

不決定 trigger、既有 Skill 選擇、Planner、Candidate 版本迭代或 evaluator gate。本子 PRD 不得接受未通過、缺少或狀態不明的 EvaluationResult，也不得改寫 sandbox 結果。

## 介面與直接依賴

依賴 SP-03 提供不可變的 gate-passed Candidate、EvaluationResult、Candidate lineage；依賴 SP-01 提供 Skill Library、dispatch、Snapshot、events、segments、reset 與 UI shell。原因：promotion 是安全閘門後的 live 行為，必須有可驗證的通過證據及可套用的 scheduler seam。

開發時兩者為 **Contract** 依賴：可用固定 passed EvaluationResult 與 deterministic Scheduler adapter 平行準備。正式 promotion／dispatch／瀏覽器驗收另為 **Integration** 依賴，須等 SP-01 與 SP-03 的實際輸出可整合。

## 必要行為

- `register_skill` 只接受 gate-passed Candidate；重複、未通過或狀態不明 input 必須拒絕且不改變 Skill Library。
- 註冊成功後 Skill Library 可查到 ID、規則、code、來源、驗證狀態與使用記錄；Demo mode 不提供人工 accept。
- `activate_skill` 記錄原因與生效時點，開新 segment；running Job 保留，下一次 dispatch 才用新 Policy。
- Live Run、policy segment、Candidate evaluation metrics 分開保存與呈現。不可用混合 live metrics 證明 Candidate 在 sandbox 外的普遍優勢。
- Adapter 操作或同步失敗時，controller 停止播放、保留最後有效 Snapshot、顯示錯誤；不能默默切 Mock 或重送有副作用呼叫。
- reset 一律回到初始四個 Skills，不保留 Candidate、promotion 或使用歷史。

## 驗收與證據

- 測試未通過 Candidate 被拒絕且 Skill Library／Live Policy 不變；通過 Candidate 被登錄後可查詢。
- 測試 registration 與 activation events、Policy／segment 記錄、running Job 保留及下一次 dispatch 生效。
- 測試 reset 清除新增 Skill、segments、usage 與 adaptation state。
- 模擬 register、activate、snapshot 失敗，驗證最後有效畫面、暫停、重新同步／reset 與禁止盲目重送。
- 瀏覽器驗證 Skill Library、Arena 與 Metrics 同一 Snapshot 的註冊與啟用證據，並完成與 SP-01、SP-03 的 team mode 整合。

## 尚待決定

Policy segment metrics 對跨切換 Job 的精確歸屬，以及額外 Skill metadata 的產品意義，尚待決定。已確認的是每個 dispatch 都有 Policy／segment 記錄，且必須顯示 segment metrics；不得自行把未定義欄位或歸屬變成 gate。
