# Adaptive Scheduler Arena：咏宸核定的產品決策

## 文件目的

本文件保留四項產品問題、咏宸的選擇及決定理由。它是決策紀錄；正式實作仍以已同步的 `DECISIONS.md`、總 PRD、相關子 PRD、後端契約與驗收矩陣為準。

## 核定摘要

- 指標惡化即暫停 simulation clock，由 AI 自動完成 adaptation；同一 run、同一 workload window 不重開另一輪。
- 成功重用或啟用 Skill 後自動恢復；五版失敗或外部錯誤時保持暫停，由使用者手動 retry、resync 或 reset。
- 跨 Policy Job 的 metrics 依 dispatch 時記錄的 Policy segment 歸屬。
- 本版維持既有最小 Skill 欄位，不增加 metadata。

## 1. 重複 adaptation trigger 如何處理？

### 白話情境

系統剛因指標變差而開始找更好的 Skill，尚未完成時，相同 workload window 又出現一次警報。若兩次 adaptation 同時執行，可能各自選出不同 Policy，造成重複花費與互相覆蓋。

### 請選一項

- [ ] **A．忽略後來的 trigger。** 第一輪執行中收到的同 window trigger 全部丟棄。最簡單，但可能漏掉新資訊。
- [ ] **B．合併為一次待處理請求（建議）。** 同一時間只跑一輪；期間若有新 trigger，只記一個 pending 標記。第一輪結束後，以最新 Snapshot 再判斷是否需要一輪。
- [ ] **C．每次都排隊。** 不並行，但每個 trigger 都保留並依序處理。資訊最完整，亦可能累積過時工作。
- [x] **D．暫停模擬器並自動完成一輪。** 指標惡化即暫停 simulation clock；AI 在背景自動比較既有 Skills，必要時產生並評估 Candidate。同一 run、同一 workload window 的重複 trigger 沿用目前 `adaptation_id`，不排隊也不重開。成功重用或啟用 Skill 後自動恢復；五版失敗或外部錯誤時保持暫停，由使用者手動 retry、resync 或 reset。

### 此決定影響

SP-01 提供 simulation clock 的 pause／resume seam；SP-02 保證一個 run／window 只有一輪 adaptation；SP-03 與 SP-04 分別處理自動迭代、成功恢復及失敗後的手動恢復。

## 2. 跨 Policy Job 的 segment metrics 歸屬為何？

### 白話情境

一筆 Job 由舊 Policy 派工，執行期間系統切換到新 Policy，最後才完成。必須決定這筆 Job 的完成與延遲算給舊區段、新區段，或拆開計算。

### 請選一項

- [x] **A．依 dispatch 時點歸屬（建議）。** Job 被派工時使用哪個 Policy，全部結果就算給該 Policy segment。規則與現有 `dispatched_policy_id`、`segment_id` 一致，最易追溯。
- [ ] **B．依完成時點歸屬。** Job 完成時哪個 Policy 生效，就算給該 segment。畫面易按時間理解，但新 Policy 可能承接舊 Policy 選出的工作。
- [ ] **C．拆分歸屬。** 等待、處理及完成結果分別計算。資訊較細，但模型、UI 與驗收成本最高。

### 此決定影響

SP-01、SP-04、Snapshot 與 Metrics UI 均依 Job 的 `dispatched_policy_id`、`segment_id` 計算；segment metrics 仍不得取代相同 baseline 的 sandbox promotion gate。

## 3. 額外 Skill metadata 有何產品意義？

### 白話情境

Skill 已有 ID、名稱、說明、code、來源、驗證狀態與使用記錄。若再加入建立者、模型、成本或標籤等欄位，必須知道使用者會如何使用；否則只會增加開發與畫面負擔。

### 請選一項

- [x] **A．維持現有最小欄位（建議）。** 本版不加 metadata；未來有明確使用情境再增補。
- [ ] **B．只加稽核欄位。** 加入建立時間、來源 Candidate、評估版本等可追溯資訊，不作排序或 gate。
- [ ] **C．加入產品分類欄位。** 另含 workload 標籤、適用範圍或品質摘要，並在 Skill Library 顯示；須同時定義資料來源與使用方式。

### 此決定影響

後端契約與 Skill Library 只實作已確認的最低欄位；未知用途不得形成 UI、排序或 gate 的必要欄位。

## 4. 外部錯誤 event 與重試 UI 如何呈現？

### 白話情境

Planner、Evaluator、Critic 或 LLM 服務可能 timeout 或回傳錯誤。錯誤時停止 adaptation、保持模擬暫停、保留最後有效畫面並記錄可見錯誤；使用者可從安全 checkpoint 手動重試。

### 請選一項

- [x] **A．記錄錯誤並提供手動重試（建議）。** UI 顯示失敗步驟與原因；模擬保持暫停，使用者確認後從可證明安全的 checkpoint 重試。狀態不明時只可重新同步或 reset。
- [ ] **B．有限自動重試後改為手動。** 只有可證明無副作用或具 idempotency 的呼叫可自動重試固定次數；其餘仍依 A 處理。
- [ ] **C．不提供重試。** 本輪 adaptation 結束，只能等待新 trigger 或 reset。最安全，但 Demo 恢復能力最低。

### 此決定影響

Event、錯誤狀態、手動 retry 按鈕及 R13／R16 必須可驗收。不得加入靜默 retry、自動切換 Mock，或在狀態不明時重送修改操作。

## 核可紀錄

- 狀態：已核定並已寫回產品權威
- 決定者：咏宸
- 日期：2026-09-12
- 決定摘要：採第一題 D、第二題 A、第三題 A、第四題 A；長時間 progression run 用於證明多輪自動演進，但不設定固定 30 分鐘，也不取代 90 秒短展示。
- 寫回產品權威的 commit／連結：由同時包含本檔與產品權威同步變更的 Git commit 追溯
