# Adaptive Scheduler Arena：待咏宸決定的產品問題

## 文件目的

本文件用白話整理四項仍會改變產品行為的問題，供咏宸決定。它是決策請求，不是產品規格；勾選前，任何選項都不得視為正式需求。決定後，文件負責人須把結果寫回 `DECISIONS.md`、總 PRD、相關子 PRD、後端契約與驗收矩陣；未寫回前，不得依本文件開始受影響的實作。

## 回覆方式

請每題勾選一項，必要時補一句理由：

```text
1. 重複 adaptation trigger：A／B／C；理由：
2. segment metrics 歸屬：A／B／C；理由：
3. 額外 Skill metadata：A／B／C；理由：
4. 外部錯誤與重試：A／B／C；理由：
決定者：
日期：
```

## 1. 重複 adaptation trigger 如何處理？

### 白話情境

系統剛因指標變差而開始找更好的 Skill，尚未完成時，相同 workload window 又出現一次警報。若兩次 adaptation 同時執行，可能各自選出不同 Policy，造成重複花費與互相覆蓋。

### 請選一項

- [ ] **A．忽略後來的 trigger。** 第一輪執行中收到的同 window trigger 全部丟棄。最簡單，但可能漏掉新資訊。
- [ ] **B．合併為一次待處理請求（建議）。** 同一時間只跑一輪；期間若有新 trigger，只記一個 pending 標記。第一輪結束後，以最新 Snapshot 再判斷是否需要一輪。
- [ ] **C．每次都排隊。** 不並行，但每個 trigger 都保留並依序處理。資訊最完整，亦可能累積過時工作。

### 此決定影響

影響 SP-02 的 trigger 狀態、event、測試、資源花費及 SP-03 何時可開始。未決前只能完成單次 trigger 路徑，不可宣稱重複 trigger 行為已驗收。

## 2. 跨 Policy Job 的 segment metrics 歸屬為何？

### 白話情境

一筆 Job 由舊 Policy 派工，執行期間系統切換到新 Policy，最後才完成。必須決定這筆 Job 的完成與延遲算給舊區段、新區段，或拆開計算。

### 請選一項

- [ ] **A．依 dispatch 時點歸屬（建議）。** Job 被派工時使用哪個 Policy，全部結果就算給該 Policy segment。規則與現有 `dispatched_policy_id`、`segment_id` 一致，最易追溯。
- [ ] **B．依完成時點歸屬。** Job 完成時哪個 Policy 生效，就算給該 segment。畫面易按時間理解，但新 Policy 可能承接舊 Policy 選出的工作。
- [ ] **C．拆分歸屬。** 等待、處理及完成結果分別計算。資訊較細，但模型、UI 與驗收成本最高。

### 此決定影響

影響 SP-01、SP-04、Snapshot、Metrics UI 及 evaluator 解讀。未決前可保存每次 dispatch 的 Policy／segment 原始記錄，但不可用 segment metrics 作 promotion gate。

## 3. 額外 Skill metadata 有何產品意義？

### 白話情境

Skill 已有 ID、名稱、說明、code、來源、驗證狀態與使用記錄。若再加入建立者、模型、成本或標籤等欄位，必須知道使用者會如何使用；否則只會增加開發與畫面負擔。

### 請選一項

- [ ] **A．維持現有最小欄位（建議）。** 本版不加 metadata；未來有明確使用情境再增補。
- [ ] **B．只加稽核欄位。** 加入建立時間、來源 Candidate、評估版本等可追溯資訊，不作排序或 gate。
- [ ] **C．加入產品分類欄位。** 另含 workload 標籤、適用範圍或品質摘要，並在 Skill Library 顯示；須同時定義資料來源與使用方式。

### 此決定影響

影響後端契約、Skill Library、保存範圍及驗收。未決前只實作已確認的最低欄位，不得為未知用途預留任意欄位。

## 4. 外部錯誤 event 與重試 UI 如何呈現？

### 白話情境

Planner、Evaluator、Critic 或 LLM 服務可能 timeout 或回傳錯誤。現有規格已要求停止 adaptation、保留最後有效畫面並顯示錯誤；尚未決定是否建立專用 event，以及使用者如何重試。

### 請選一項

- [ ] **A．記錄錯誤並提供手動重試（建議）。** UI 顯示失敗步驟與原因；使用者確認後，從可證明安全的 checkpoint 重試。狀態不明時只可重新同步或 reset。
- [ ] **B．有限自動重試後改為手動。** 只有可證明無副作用或具 idempotency 的呼叫可自動重試固定次數；其餘仍依 A 處理。
- [ ] **C．不提供重試。** 本輪 adaptation 結束，只能等待新 trigger 或 reset。最安全，但 Demo 恢復能力最低。

### 此決定影響

影響 Event 類型、錯誤狀態、按鈕、成本、重複呼叫安全及 R13／R16 驗收。未決前不得加入靜默 retry 或自動切換 Mock。

## 核可紀錄

- 狀態：待咏宸逐題決定
- 決定者：待填
- 日期：待填
- 決定摘要：待填
- 寫回產品權威的 commit／連結：待填
