# build-handoff_final 共用決策摘要

## 文件角色

本頁供建立及驗收 Session 快速取得已確認決策。它不是產品或流程權威。若本頁與來源衝突，先依下列權威順序判斷；仍無法唯一判斷時，標示為「尚待決定」。

## 權威順序

1. `discussion_record/` 中較新的明確團隊決定，高於較舊的討論方案。
2. 題目、產品範圍、功能行為及技術內容以 `build-handoff_2/` 為最新版來源。
3. 工作流程以 `docs/playbook/workflow.md` 及 `docs/operations/team-work-allocation.md` 為正式基準。
4. `build-handoff/` 提供流程落地方式及既有文件結構，不再提供現行產品內容。
5. `build-handoff_2/tickets/agent-orchestration/AO-001.md` 至 `AO-010.md` 是咏宸的拆解草案，不是已核准 Ticket。

題目已獲准更換。若較舊文件仍寫「等待主辦方核准」，視為待清理的過時狀態，不可覆蓋最新決定。

## 已確認的工作方式

- `build-handoff_final/` 是整理後的單一 handoff，不是兩個來源目錄的機械聯集。
- 建立期間保留 `build-handoff/` 與 `build-handoff_2/` 原狀。
- `build-handoff_final/` 通過獨立驗收前，只是候選 handoff，不得宣稱已正式取代 `build-handoff/`。
- 任何參與者都可成為子 PRD Owner。比賽當日由認領該子 PRD 的人擔任。
- 賽前不指定 Owner、不建立 GitHub Issue、不建立正式 Ticket。
- 比賽當日由 Owner 建立子 PRD 的 GitHub parent issue，再依已核准子 PRD 建立 Ticket。
- 子 PRD 是一項可獨立展示、測試及驗收的完整功能，不以單一技術元件作水平切割。
- 每份子 PRD 以 2 至 3 張 vertical-slice Ticket 為標準。
- 預估需要 4 至 5 張 Ticket 時，須重新檢查邊界、記錄不拆理由並取得特別核准。
- 預估超過 5 張 Ticket 時，不得核准；必須拆分子 PRD 或正式縮減範圍。
- 工作依真正依賴順序進行，不以編號代替依賴判斷。
- 子 PRD 完成須包含整體功能驗收、測試、AI review、Owner 自查、另一位成員確認、正式 PR merge 及 `main` 整合驗收。另一位確認者不預先指定。
- AO-001 至 AO-010 留在 `build-handoff_2/` 作參考，不搬入 final。

## 待產品決定

重複 adaptation trigger、跨 Policy Job 的 segment metrics 歸屬、額外 Skill metadata，以及外部錯誤 event／重試 UI，整理於 [產品決策簡報](PRODUCT-DECISION-BRIEF.md)，待咏宸逐題決定。該簡報只提出選項與影響，不是產品權威；決定寫回總 PRD、相關子 PRD、後端契約與驗收矩陣後，才可作為實作基準。

## 子 PRD 候選

目前較合理的四個候選為：

1. `scheduling-arena.md`
2. `existing-skill-adaptation.md`
3. `candidate-evaluation-loop.md`
4. `skill-promotion-and-recovery.md`

這些名稱不是預先核准的最終切法。建立 Session 必須以 `build-handoff_2/` 的完整需求驗證：

- 每份是否形成完整、可觀察的交付結果。
- 每份是否合理預估為 2 至 3 張 vertical-slice Ticket。
- 四份合計是否覆蓋總 PRD，沒有功能缺口或責任重複。
- Candidate evaluation 與 promotion/recovery 是否具有明確的安全及資料契約邊界。

Scheduler、Agent Orchestration、Sandbox Evaluation、Skill Library 與 Demo UI 可作技術責任或 contract 分類，不必各自成為子 PRD。

## Demo UI

Demo UI 原則上分配到各功能子 PRD：

- 排程及 workload 操作屬 scheduling arena。
- adaptation、evaluation、promotion、錯誤及 fallback 的可見證據屬對應功能子 PRD。
- 只有一組 Demo 體驗能獨立交付，且合理需要 2 至 3 張 Ticket 時，才建立 `demo-experience.md`。
- 若只是全案串接及展示確認，納入 `ACCEPTANCE.md`，不新增子 PRD。

## DEPENDENCIES.md

`sub-PRDs/DEPENDENCIES.md` 只作跨子 PRD 的靜態依賴索引：

- 固定以「Consumer depends on Provider」表示方向。
- 依賴類型使用 Hard、Contract、Integration、None。
- 每條 edge 至少記錄：Edge ID、Consumer、Provider、類型、所需輸出或 contract、可開始條件、整合條件、Provider 尚未完成時的替代方式，以及例外核准方式。
- 每份子 PRD 本身仍是其直接依賴及理由的權威。
- 中央索引與子 PRD 衝突時，停止相關整合，由 main branch 負責人處理。
- 不在此檔記錄 Owner、即時狀態、GitHub Issue 編號、Ticket、進度或測試結果。

需求 coverage 不放入 `DEPENDENCIES.md`。`ACCEPTANCE.md` 應另列總 PRD 需求、負責子 PRD 與驗收證據的對照。

## 預期文件

建立 Session 應評估並產生最小充分集合：

```text
build-handoff_final/
├── README.md
├── AGENTS.md
├── INDEXER.md
├── CONTEXT.md                  # 僅在不重複 PRD 且確有用途時建立
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
│   ├── sub-prd.md
│   ├── parent-issue.md
│   └── ticket.md
└── sub-PRDs/
    ├── DEPENDENCIES.md
    └── <核實後的子 PRD 檔案>
```

賽前不建立 `tickets/`、`drafts/`、`STATUS.md`、`OPEN_QUESTIONS.md`、`MIGRATION.md` 或 `source-prds/`。未決事項應放入其所影響的權威文件，不另建重複清單。

## 可在建立時解決的問題

可直接處理：

- 能由權威文件及最新決策唯一推導的答案。
- 檔名、路徑、索引、內部連結及重複內容等編輯性問題。
- 讓 final 自足且不依賴 planning repository 才能理解的必要調整。

必須保留為「尚待決定」：

- 存在兩種以上合理答案，且會改變產品範圍、可觀察行為、驗收門檻、安全邊界或人員權責。
- 來源沒有給出可驗證答案。
- 解答需要主辦方或團隊的新決策。

`CONTEXT.md` 是否存在，以及是否新增 `demo-experience.md`，須由建立 Session 依上述條件判斷，不以湊齊目錄為目的。

## 主要討論來源

- `discussion_record/2026-09-11-build-handoff-merge-direction.md`
- `discussion_record/2026-09-12-sub-prd-preparation-and-event-day-tickets.md`
- `discussion_record/2026-09-12-build-handoff-final-file-plan.md`
- `discussion_record/2026-09-12-sub-prd-source-placement.md`
- `discussion_record/2026-09-12-two-sub-prd-vertical-slice-review.md`
- `discussion_record/2026-09-12-sub-prd-count-and-completeness.md`
- `discussion_record/2026-09-12-sub-prd-ticket-cap-and-candidate-split.md`
- `discussion_record/2026-09-12-dependencies-and-final-open-file-plan.md`
