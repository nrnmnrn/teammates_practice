# build-handoff_final 檔案規劃討論紀錄

- 日期：2026-09-12
- 性質：對話紀錄，供後續討論追蹤
- 權威限制：本文件不是總 PRD、子 PRD、Ticket、核准紀錄或流程規範。若與正式文件衝突，以正式文件及最新團隊決議為準。

## 本輪由于喬確認的方向

1. 同意上一輪關於賽前子 PRD準備、比賽當日認領及建立 Ticket 的方向。
2. 新建 `build-handoff_final/` 作為 `build-handoff/` 與 `build-handoff_2/` 的整合成果。
3. `build-handoff_final/` 保留 `build-handoff/` 的工作流程。
4. `build-handoff_final/` 採用 `build-handoff_2/` 的完整題目與產品內容。
5. 本輪只規劃應搬入及新建的檔案，不建立或修改 `build-handoff_final/`。

## 建議的目錄骨架

```text
build-handoff_final/
├── README.md
├── AGENTS.md
├── INDEXER.md
├── CONTEXT.md
├── PRD.md
├── ACCEPTANCE.md
├── workflow.md
├── sub-prd-authoring.md
├── docs/
│   ├── adr/
│   │   ├── 0001-document-authority.md
│   │   └── 0002-one-branch-per-sub-prd.md
│   └── contracts/
│       └── backend-contract.md
├── templates/
│   └── sub-prd.md
└── sub-PRDs/
    ├── DEPENDENCIES.md
    ├── agent-orchestration.md
    ├── demo-ui.md
    └── 其他經總 PRD 邊界分析確認的子 PRD
```

賽前不建立 `tickets/`。AO-001 至 AO-010 不搬入 final，避免草案被誤認為正式任務。

## 可由 build-handoff 搬入並保留主體的檔案

- `workflow.md`：保留 Ticket、branch、review、merge 與 closeout 流程；修正 final 內路徑及已過時狀態。
- `sub-prd-authoring.md`：作為子 PRD 編寫與核准規則。
- `templates/sub-prd.md`：作為所有正式子 PRD 的共同模板，加入賽前 `Unclaimed` 與 GitHub issue 待建立的填法。
- `docs/adr/0001-document-authority.md`：保留文件權威原則，更新成 final 的實際文件層級。
- `docs/adr/0002-one-branch-per-sub-prd.md`：保留一個子 PRD 對應一個 branch／Draft PR 的決策。

這些檔案不可盲目原樣複製。至少要移除「仍等待主辦方確認能否換題」等已過時敘述，並核對所有相對路徑。

## 以 build-handoff 為骨架但需重寫的檔案

- `README.md`：說明 final 是唯一 handoff、產品已確定以 `_2` 為來源、賽前不建 Ticket。
- `AGENTS.md`：保留權限與安全界線，改成新總 PRD、`sub-PRDs/` 與比賽當日認領流程。
- `INDEXER.md`：加入 `sub-PRDs/DEPENDENCIES.md`、backend contract 及新的子 PRD 讀取路徑。
- `CONTEXT.md`：以 `_2` 的產品名詞、事件、模式及系統邊界重建；不保留已被新版取代的產品假設。
- `ACCEPTANCE.md`：保留證據表用途，但驗收項目須依 `_2` 的新總 PRD 與各子 PRD 重建。

## 可由 build-handoff_2 搬入作為內容基礎的檔案

- `PRD.md`：作為 final 總 PRD 的主要內容來源；修正路徑、文件權威、子 PRD 列表及賽前／賽中狀態。
- `backend-contract.md`：移至 `docs/contracts/backend-contract.md`；保留介面內容，明定其從屬於總 PRD及相關子 PRD。

兩者仍需一致性整理，不宜逐字複製後即宣告完成。

## 不可直接搬入，應重製成正式子 PRD 的檔案

- `prd/AGENT_ORCHESTRATION_PRD.md`：以其產品內容為來源，依 `sub-prd-authoring.md` 與模板重製為 `sub-PRDs/agent-orchestration.md`。
- `prd/Hackathon_DEMO_UI_PRD.md`：以其 UI、Simulator、Adapter、Metrics 與驗收內容為來源，重新確認邊界後製成 `sub-PRDs/demo-ui.md`。

重製時須補 Deliverable、Scope、Out of scope、總 PRD 關係、Inputs／Outputs、Dependencies、Acceptance、Owner、GitHub parent issue、Approval record 與 Open questions。

## 必須新建的檔案

### `sub-PRDs/DEPENDENCIES.md`

集中列出子 PRD 的直接依賴、所需輸出、可開始條件、可否平行及例外。此檔只作導航與排程索引，不另創需求。

### 正式子 PRD 檔案

至少需重製：

- `sub-PRDs/agent-orchestration.md`
- `sub-PRDs/demo-ui.md`

總 PRD 尚含 Scheduler Backend、Sandbox Evaluation、Skill Library 等能力。是否另建子 PRD，須先依「完整功能、可獨立驗收、可獨立合併」檢查，不能只因它們是技術元件便各建一份。

## 不搬入 final 的檔案

- `build-handoff/PRD.md`：舊產品內容已由 `_2/PRD.md` 取代。
- `build-handoff_2/tickets/agent-orchestration/README.md`
- `build-handoff_2/tickets/agent-orchestration/AO-001.md` 至 `AO-010.md`

AO 檔仍保留在 `_2` 作參考。比賽當日由實際 Owner 依正式子 PRD建立 GitHub Ticket。

## 建議的建立順序

1. 建立 final 目錄骨架。
2. 建立唯一總 PRD。
3. 更新文件權威、README、AGENTS 與 INDEXER。
4. 建立子 PRD清單及 `DEPENDENCIES.md`。
5. 逐份重製正式子 PRD。
6. 定位 backend contract。
7. 依新規格重建 ACCEPTANCE。
8. 檢查所有連結、權威、依賴與過時敘述。
9. 停止；不建立 Ticket、不預排 Owner。

## 本輪結論

`build-handoff_final/` 不應是兩個目錄的聯集。它應是經過取捨的單一 handoff：流程資產取自 `build-handoff/`，產品內容取自 `build-handoff_2/`，正式子 PRD重新製作，AO 草案不納入。
