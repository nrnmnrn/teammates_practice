# 子 PRD 工作清單

完整說明請看[四人協作政策](../operations/team-work-allocation.md)。先記住：**總 PRD 管整體、子 PRD 管完整功能、Ticket 管一次工作。**

## 誰做哪一段

- 于喬平常處理總 PRD 或模糊的新工作：`$grill-me` 或 `$grill-with-docs` → `$to-spec` → `$to-tickets` → `$implement`。
- 其他組員承接已核准的子 PRD：子 PRD 就是 spec，先 `$to-tickets`，再對每張 Ticket 執行 `$implement`。
- 每張 Ticket 真正完成後：先 `/compact`，再 `$task-closeout`。`$implement` 已 commit 時，closeout 用來核對狀態與下一步，不再重複 commit。
- 少見或特殊情況下，于喬若明確指派組員從頭負責完整功能，該組員才參考于喬的完整流程。

## 接工作前

- [ ] 我讀過總 PRD、完整子 PRD 與目前 Ticket。
- [ ] 子 PRD 說清楚交付結果、範圍、不做範圍、依賴、驗收方式、owner 與未解問題。
- [ ] 于喬已確認我承接這一整份子 PRD。
- [ ] 前置成果已在 `main` 可用；若未可用，已取得于喬的明確例外同意。
- [ ] 子 PRD 有一條 branch 與一個 Draft PR。

> **備注：什麼是「前置成果已在 `main` 可用」？**
> 這張 Ticket 所依賴的其他子 PRD 成果，已合併進團隊共用的正式版本 `main`，且確認可用。例如畫面要顯示模擬結果前，模擬器必須先能產生正確結果。同一份子 PRD 的下一張 Ticket 可直接使用同一條 branch 中前一張 Ticket 的成果，不必每張 Ticket 都先合併到 `main`。
>
> **備注：什麼是 Draft PR？**
> Draft Pull Request 是「仍在施工」的變更單。它讓隊友看見子 PRD 的進度，但尚未能合併。每份子 PRD 只開一個 Draft PR，所有 Tickets 的 commits 都放進去；子 PRD 全部完成、通過完整檢查與人工審查後，才轉為正式 PR 並 merge。

## 每次 agent session

- [ ] 讀總 PRD 共用規則、完整子 PRD、目前 Ticket、前置證據、最新 `main` 與未解紀錄。
- [ ] 只做一張目前可執行的 Ticket。
- [ ] 若發現文件矛盾或題意不清，停下來回報于喬；需要時請咏宸核對。
- [ ] 若同一 blocker 超過 15 分鐘，或兩次有證據嘗試仍無法解決，標為 Blocked。
- [ ] 完成時留下驗收與測試證據、清楚 commit、未解問題、下一張可執行 Ticket。

> **備注：什麼是前置證據？**
> 它不是只指上一張 Ticket，而是「我現在可安全開始」的證明。可能是前一張 Ticket 的 commit 與測試結果、其他子 PRD 已 merge 到 `main` 的紀錄、于喬明確同意先依穩定介面並行，或本 Ticket 沒有前置依賴。

## 一張 Ticket 做完才算完成

- [ ] 驗收條件全部通過。
- [ ] 相關測試通過。
- [ ] 執行 `$implement`，完成其要求的 AI `/code-review` 與 commit。
- [ ] 記錄限制或 blocker。

每張 Ticket 不要求人工 review 或 merge。不要等待整份子 PRD 做完才第一次檢查成果；遇到共用介面或整合點，請于喬快速確認。

## 子 PRD 做完才關閉

- [ ] 全部 Tickets 已完成，且整個功能真的可以使用。
- [ ] 完整測試、最後 AI review、owner 自查均完成。
- [ ] 于喬完成架構、範圍與整合 review。
- [ ] 高風險結果已由咏宸核對。
- [ ] 正式 PR merge 後，已在 `main` 驗收整合結果。

## 變更規則

- 原本功能做錯或漏一步：原子 PRD 補 Ticket。
- 新增獨立功能：于喬新增子 PRD 與 Tickets。
- 不清楚或衝突：停止，先把規格寫清楚。

## 賽前限制

本 planning repo 不寫比賽程式。官方 coding window 開始後，才在未來 competition implementation repo 開發；`build-handoff/` 是唯一交接介面。

正式開始前，只在獨立、可丟棄的非比賽 toy／sandbox repo 演練此流程。演練不可含比賽產品程式或可重用實作。CI 需在演練後取得使用者明確同意；CD 不列為預設。
