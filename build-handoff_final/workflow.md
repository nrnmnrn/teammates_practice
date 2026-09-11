# 子 PRD 工作流程

本流程於官方 coding window 開始後，在實作 repository 使用。總 PRD 管產品；子 PRD 管一項可獨立展示、測試與驗收的完整功能；Ticket 管一次可獨立驗證的工作。詳見 [文件權威 ADR](docs/adr/0001-document-authority.md)。

## 1. 開始與認領

開始前確認：

- 該子 PRD 已核可，且交付結果、範圍／不做範圍、PRD coverage、直接依賴與驗收都完整。
- 官方 coding window 已開始。賽前不預排 Owner，不建立 GitHub issue 或 Ticket。
- 認領人即為 Owner，建立該子 PRD 的 GitHub parent issue，再依核可子 PRD 建立 2 至 3 張 Ticket；使用 [parent issue 模板](templates/parent-issue.md) 與 [Ticket 模板](templates/ticket.md)。
- Owner 由最新 `main` 建立此子 PRD 唯一的 branch 與 Draft PR，並把 branch、PR 與規格版本連回 parent issue。
- 已確認直接依賴。依賴一律寫成「Consumer depends on Provider」。Provider 的成果通常必須已 merge 並在 `main` 驗證；例外須由 main branch 負責人明示並記在 parent issue。

main branch 負責人是人類整合與跨功能決策角色；AI 不得自行擔任、核可或 merge。

## 2. 執行一張 Ticket

每個 session 只處理一張目前可開始的 Ticket：

1. 讀 PRD 的共用規則、完整子 PRD、目前 Ticket、parent issue、直接依賴證據、最新 `main` 與未解問題。
2. 核對 Ticket 的開始條件與驗收條件。依真正依賴順序工作，不以 Ticket 編號推定先後。
3. 在該子 PRD branch 完成這一張 Ticket，執行相關測試與工作者自驗。
4. 完成 AI `/code-review`，處理必要發現；AI 提供檢查，不提供人工核可。
5. 只有驗收與測試通過時才建立清楚的 Ticket commit，並在 parent issue 留下 commit、測試／驗收證據、限制與下一張可開始的 Ticket。
6. 執行 closeout：核對 Ticket 真的完成、狀態與證據一致，並記錄下一步。closeout 不另建重複 commit。

一張 Ticket 的完成，不等於整個子 PRD 已完成。每張 Ticket 不需要單獨人工 merge 或共同核可。

## 3. 阻塞與變更

同一 blocker 超過 15 分鐘，或已完成兩次有證據的嘗試仍不能前進時，在 parent issue 標記 `Blocked`，記錄症狀、嘗試、證據與需要的決定，並通知 main branch 負責人。

規格有衝突、題意不清或跨功能介面不一致時，停止受影響工作。原範圍確有遺漏時可提案補 Ticket，但必須仍符合 [sub-PRD 撰寫規則](sub-prd-authoring.md) 的 Ticket 數量閘門；改變交付結果、範圍、驗收或直接依賴，必須先更新並核可子 PRD，不能只改 issue。

## 4. 子 PRD 完成、merge 與結案

所有 Ticket 完成後，依序完成下列關卡：

1. Owner 驗收整個功能而非僅檢查 Ticket 狀態，執行完整相關測試與最後 AI review，並完成 Owner 自查。
2. 至少另一位成員實際確認功能、證據與直接整合結果；該確認者不預先指定。
3. main branch 負責人檢查範圍、架構、依賴與整合條件。取得人類合併授權後，將 Draft PR 轉為正式 PR 並 merge。
4. 在 `main` 執行整合驗收，確認此功能與已整合功能一起正常運作。
5. 將 `main` 驗收證據寫入 parent issue，才可關閉 parent issue 並完成該子 PRD 的 closeout。

整體完成還需要 [ACCEPTANCE.md](ACCEPTANCE.md) 所列所有功能在同一份 `main` 上通過整合驗收與展示確認。
