# 子 PRD 準備與比賽當日 Ticket 流程討論紀錄

- 日期：2026-09-12
- 性質：對話紀錄，供後續討論追蹤
- 權威限制：本文件不是總 PRD、子 PRD、Ticket、核准紀錄或流程規範。若與正式文件衝突，以正式文件及最新團隊決議為準。

## 延續決策

- 產品與題目內容以 `build-handoff_2/` 為最新版。
- 工作流程以 `docs/playbook/workflow.md` 與 `docs/operations/team-work-allocation.md` 為準。
- `build-handoff/` 是未來 implementation repo 的 handoff 介面。
- 子 PRD 不預先分配固定 Owner；比賽當日由取得者認領。
- AO-001 至 AO-010 是咏宸的拆解草案，不是正式 Ticket。

## 本輪由于喬提出的方向

1. 暫緩審查 AO 草案，避免現階段產生過多任務。
2. 比賽前不先建立正式 Ticket。
3. 現階段主要工作是把 `_2` 的產品內容重新拆成符合流程的子 PRD。
4. 每個子 PRD 存為 `build-handoff/sub-PRDs/` 內的獨立檔案。
5. 子 PRD 依 `build-handoff/docs/sub-prd-authoring.md` 製作。
6. `sub-PRDs/` 另設一份文件，集中記錄各子 PRD 的依賴關係。
7. 比賽當日依子 PRD 與依賴表建立 GitHub parent issue。
8. 認領子 PRD 的 Owner 再自行建立其下 Ticket。
9. Owner 於比賽當日決定；賽前不預排，以免現場計畫改變。
10. 不預先處理由哪一位成員負責功能確認。

## 對此方向的判斷

### AO 草案暫緩

同意暫緩。AO 草案已保存在 `_2`，資訊不會消失。現階段若逐張審查，容易把尚未完成的子 PRD 邊界固化成執行計畫。

AO 草案目前只作參考材料。正式 Ticket 應在父層子 PRD 核准、Owner 認領、現場條件確認後才建立。

### 比賽前不建 Ticket

同意。此舉符合「先有完整且核准的子 PRD，再拆 Ticket」的流程。亦可避免題目、時間、工具或人力改變後留下失效 Ticket。

比賽前的完成標準應限於：總 PRD 明確、子 PRD 完整、依賴可讀、未決事項有標示。不得把草案檔案數量當成進度。

### Owner 延後認領

同意不預先指派。子 PRD 仍應保留 Owner 欄位，賽前填 `Unclaimed` 或等義文字。比賽當日取得者再填入實際 Owner，並於 GitHub parent issue 留下可追溯紀錄。

### 功能確認角色

可不預先決定具名確認者，但不可刪除確認關卡。現行流程要求子 PRD 完成後，由另一位成員確認完整功能。比賽當日 closeout 時再指定即可。

## 建議的 `sub-PRDs/` 職責

每個檔案代表一個可獨立驗收、可獨立合併的完整功能，不代表一個程式模組，也不代表單一工作步驟。

每份子 PRD 至少包含：

- Deliverable
- Scope
- Out of scope
- 與總 PRD 的關係
- Inputs／Outputs
- Dependencies
- Acceptance
- Owner
- GitHub parent issue
- Approval record
- Open questions

賽前可將 Owner 與 GitHub parent issue 標為未認領／待建立。若仍有會改變實作方向的未決事項，子 PRD不得標示為可開工。

## 依賴關係檔案的建議界線

集中依賴表適合作為導航及排程索引，但不應成為第二份需求來源。

建議只記錄：

- 子 PRD ID 與名稱
- 直接上游子 PRD
- 所需輸出或 contract
- 可開始條件
- 是否可平行
- 尚待確認的依賴例外

每份子 PRD 仍須自行列出直接依賴。集中表若與子 PRD 衝突，應停止並修正文檔，不應讓執行者自行猜測。

## 比賽當日建議流程

1. 團隊檢查現場規則、時間及可用工具是否改變。
2. 成員依依賴表選取可開始的子 PRD。
3. 取得者成為 Owner，確認子 PRD 內容及前置條件。
4. 依子 PRD 建立 GitHub parent issue，記錄 Owner、版本及驗收範圍。
5. Owner 使用正式流程把該子 PRD拆成當日有效的 Ticket。
6. 每次只執行一張 Ticket，留下測試、review、commit 與未決事項。
7. 所有 Ticket 完成後，再做子 PRD 整體驗收及另一位成員確認。

## 尚待後續討論

- `_2` 的完整產品應拆成哪些「完整功能」子 PRD；不得直接沿用技術元件名稱作結論。
- 集中依賴表的正式檔名及欄位。
- 哪些共用 contract 是子 PRD 的輸出，哪些只是總 PRD 的共同限制。
- 子 PRD 在賽前可達到何種核准狀態，以及比賽當日何時轉為可開工。
- AO 草案在最終 handoff 中保留、移至參考區，或不納入；目前待定。

## 本輪結論

現階段聚焦總 PRD、正確子 PRD 與依賴索引。Ticket、Owner 及具名功能確認者延至比賽當日。AO 草案暫不審查。功能確認關卡仍依正式流程保留，但無須現在指定人選。
