# Build handoff 合併方向討論紀錄

- 日期：2026-09-11
- 性質：對話紀錄，供後續討論追蹤
- 權威限制：本文件不是總 PRD、子 PRD、Ticket、核准紀錄或流程規範。若與正式文件衝突，以正式文件及最新團隊決議為準。

## 對話背景

目前有兩套 handoff 文件：

- `build-handoff/` 由于喬主導，工作流程、文件權威、驗收、review、merge 與 closeout 較完整。
- `build-handoff_2/` 由咏宸更新，題目內容、Agent Orchestration、UI、backend contract 與工作拆解較完整，但流程治理不完整。

前一輪唯讀比較所得：

- `build-handoff/` 適合作為工作流程與治理骨架。
- `build-handoff_2/` 適合作為最新版產品內容來源。
- 不宜直接把兩個目錄全部疊加，否則可能同時存在兩套總 PRD、文件權威與工作啟動方式。
- `build-handoff_2/tickets/agent-orchestration/AO-001.md` 至 `AO-010.md` 已有具體工作拆解，但尚未證明是正式 Ticket。

## 本輪由于喬確認的決策

1. 子 PRD 不設固定 Owner。任何人皆可成為 Owner，由取得該子 PRD 的人決定。
2. 主辦方已確認可以更換題目。
3. 題目與產品內容全部以 `build-handoff_2/` 為主；該目錄是咏宸提供的最新版。
4. AO-001 至 AO-010 是咏宸的拆解草案，不是已核准的正式 Ticket。
5. 整體工作流程仍以 `docs/playbook/workflow.md` 與 `docs/operations/team-work-allocation.md` 為準。
6. 預定方向是把 `build-handoff_2/` 的完整題目資訊，納入具有完整流程的 `build-handoff/`。
7. 本輪只記錄與提出建議，不修改兩套 handoff 文件，不建立 Ticket。

## 對上述決策的理解

文件角色可分成兩條權威軸：

- 產品內容權威：以 `build-handoff_2/` 的最新版題目為來源。
- 工作流程權威：以兩份 repository 基準流程文件為來源，再由 `build-handoff/` 承載成可交付的 handoff。

因此，後續工作不是判斷哪個資料夾整體勝出，而是把兩者按職責整合：

- 保留 `build-handoff/` 的流程控制、導覽、權限、驗收證據、review 與 closeout 結構。
- 以 `build-handoff_2/PRD.md` 更新總 PRD 的產品內容。
- 以 `build-handoff_2/prd/` 的文件建立或更新正式子 PRD，但仍須補齊流程要求的欄位與核准狀態。
- 把 `build-handoff_2/backend-contract.md` 視為介面契約，明定其從屬於總 PRD 與相關子 PRD。
- 把 AO-001 至 AO-010 保留為 Ticket 候選；待其所屬子 PRD 被認領、檢查並核准後，才轉為正式 Ticket。

## 目前建議

### 先處理權威，再搬內容

合併前應明文確定：

1. `build-handoff_2/PRD.md` 是產品內容來源。
2. `docs/playbook/workflow.md` 與 `docs/operations/team-work-allocation.md` 是流程來源。
3. `build-handoff/` 是唯一輸出到未來 implementation repo 的 handoff 介面。
4. discussion record 只保存討論脈絡，不可覆蓋上述權威文件。

### Owner 採動態認領

子 PRD 模板仍應保留 Owner 欄位，但初始可標示為未認領。取得子 PRD 的人填入自己，並留下可追溯的認領或核准紀錄。這符合「任何人皆可當 Owner」，也避免實作開始後無人負責整體驗收。

### AO 拆解須延後正式化

AO-001 至 AO-010 可用於檢查工作順序、依賴與工作量，但目前只屬草案。正式化前至少需要：

- 確認其父層 Agent Orchestration 子 PRD 已被認領及核准。
- 逐項檢查 Ticket 是否仍符合最新版總 PRD。
- 補上實際 Owner、驗收方式與必要前置證據。
- 依正式流程建立 Ticket，而不是把 Markdown 草案直接當作進行中的工作狀態。

### 舊產品內容不得反向覆蓋最新版

既然團隊已指定 `_2` 為最新版，舊 `build-handoff/PRD.md` 中與 `_2` 衝突的產品內容，不應再用來否決 `_2`。舊文件仍可供追蹤變更原因，但不可與新總 PRD 同時被標示為現行產品權威。

## 尚待後續執行時確認

- 最終 `build-handoff/PRD.md` 如何標示版本、日期與取代關係。
- Agent Orchestration 與 UI 是否就是第一批正式子 PRD，或尚有其他完整功能需要拆分。
- 誰先認領各子 PRD，以及核准紀錄存放位置。
- AO-001 至 AO-010 是否全數採用，或經子 PRD 驗證後調整、合併或刪除。
- backend contract 的變更由哪個子 PRD 負責驗收。
- 合併完成後，如何證明舊產品敘述已不再具有現行權威。

## 本輪結論

採「產品用 `_2`、流程用基準文件、交付介面用 `build-handoff/`」三層分工。此方向可行，且比直接選一個目錄取代另一個更穩健。下一步若獲授權修改，應先整理權威與取代關係，再處理總 PRD、子 PRD，最後才處理 Ticket 草案。
