# child PRD 工作快速清單

完整政策見 [四人 child PRD 協作政策](../operations/team-work-allocation.md)。本頁只提供執行順序；衝突時以該政策與 parent PRD 為準。

competition implementation workflow 只在官方 coding window 開始後的 future competition implementation repo 執行。本 planning repo 賽前不建立比賽 source code；`build-handoff/` 是唯一轉交介面。賽前四人 branch、Draft PR、review、merge 演練只在獨立、可丟棄的非比賽 toy／sandbox repo 執行，不在本 planning repo 或 future competition implementation repo 執行。

## 0. 先確認可開始

- [ ] 權威順序無衝突：parent PRD、已核准 child PRD、Ticket。
- [ ] child PRD 是 versioned repo Markdown；GitHub parent Issue 有連結。
- [ ] child PRD 已有結果、範圍／不做範圍、parent 關係、contracts、依賴、驗收／demo、owner／reviewer、未解問題。
- [ ] 關鍵決策沒有缺漏；若有，先澄清，不拆 Ticket。
- [ ] Ticket 有 Parent reference，並依 blocking edge 可執行。
- [ ] 林于喬已確認 owner；一份 child PRD 只有一位 primary writer。

## 1. 拆解與認領

有歧義的新 parent 工作：`$grill-me` 或 `$grill-with-docs`，接著 `$to-spec`、`$to-tickets`、`$implement`。完整且已核准的 child PRD：`$to-tickets`，每張 Ticket 各執行一次 `$implement`。拆解中遇到歧義，回到澄清。

- [ ] child PRD 通常為 2–5 張 Ticket，且能在 integration cutoff 前完成。
- [ ] 每張 Ticket 設 30–60 分鐘目標（若既有文件有此要求）。
- [ ] 未認領者提議接整份 child PRD，不任意挑 Ticket。
- [ ] 狀態只用 Draft、Ready、Active、Blocked、Review、Done。

## 2. 每次 session

- [ ] 只處理一張 Ticket。
- [ ] 讀 parent 共同規則、完整 child PRD、Ticket、已完成 blocker 證據、最新 `main`／integration、未解紀錄。
- [ ] 卡在同一 blocker 15 分鐘或兩次有證據嘗試後，標記 Blocked 並回報。
- [ ] 若規格衝突，停止；林于喬更新規格／Ticket，咏宸核對領域問題。
- [ ] 完成時記錄 Ticket、驗收／測試證據、決策、未解問題、下一張可執行 Ticket。

## 3. 提交與 review

- [ ] child PRD 僅一條 branch、一個 Draft PR；每張 Ticket 只有一個清楚 commit。
- [ ] 只有 primary writer 寫實作；協助者若要實作，先正式轉交，或用隔離例外 branch。
- [ ] 執行所有相關本地測試與驗收。
- [ ] 執行 `$implement-required` 的輕量 AI `/code-review`；AI 不可核准或 merge。
- [ ] 記錄限制與 blocker。每張 Ticket 不需要人類 review 或 merge。
- [ ] 依賴／整合節點同步 `main`；下游只在 blocker merge 並於 `main` 驗證後開始，除非林于喬明確核准穩定介面例外。

## 4. 關閉 child PRD 與整體工作

- [ ] child PRD：完整測試、最後 AI review、owner 自查、林于喬架構／範圍／整合 review。
- [ ] 高風險結果另有咏宸領域 review；低風險交指定人類 reviewer。
- [ ] 正式 PR 已 merge，並完成 merge 後整合驗收。
- [ ] 全部 child PRD 在同一 `main` 為 Done；測試、demo、fallback 都已驗證。
- [ ] 林于喬核准整合，咏宸確認領域正確性。

## 5. 特殊情況

- 已承諾行為錯誤或漏工作：回原 child PRD 新增 Ticket。
- 新的獨立交付：林于喬建新 child PRD 和 Tickets。
- GitHub 故障：暫以一張 Ticket 一個本地檔案，沿用相同識別與欄位；恢復後同步一次，避免雙重真相。
- CI 只在演練後且使用者明確同意才加；CD 非預設。
- 四人正式開始前，在獨立、可丟棄的非比賽 toy／sandbox repo 完成一次小型假 child PRD 演練，記錄時間與 blocker；不得在本 planning repo 或 future competition implementation repo 演練，且不得含比賽 product code、可重用 implementation、RL pipeline、Agent loop 或 reward logic。competition implementation workflow 須待官方 coding window 開始。
