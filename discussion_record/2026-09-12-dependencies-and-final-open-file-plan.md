# DEPENDENCIES 與 final 尚待確定檔案討論紀錄

- 日期：2026-09-12
- 性質：對話紀錄，供後續討論追蹤
- 權威限制：本文件不是總 PRD、子 PRD、Ticket、核准紀錄或流程規範。若與正式文件衝突，以正式文件及最新團隊決議為準。

## 本輪由于喬確認的決策

- 每份子 PRD以 2～3 張 Ticket 為標準。
- 預估 4～5 張時，須重新檢查邊界、記錄不拆理由並取得特別核准。
- 超過 5 張時，不得核准；須重拆或正式縮減範圍。
- 詢問 `sub-PRDs/DEPENDENCIES.md` 的適當設計。
- 詢問 `build-handoff_final/` 還有哪些必要但尚未確定的檔案或資料夾。
- 本輪先討論及記錄，不修改 handoff 或建立 final。

## `DEPENDENCIES.md` 的定位

此檔只記子 PRD間的靜態依賴與整合閘門。它不記：

- Owner。
- 即時狀態。
- GitHub issue number。
- Ticket 清單。
- 實作進度。
- 測試結果。

上述易變資訊由 GitHub parent issue、Ticket 與 `ACCEPTANCE.md` 負責。依賴檔不可成為第二份狀態追蹤器。

## 建議結構

### 1. 文件權威與方向

明定：

- 每份子 PRD是自身直接依賴與理由的權威。
- `DEPENDENCIES.md` 是跨子 PRD導覽與一致性索引。
- 若集中表與子 PRD衝突，停止開工並交由 main branch 負責人處理。
- Edge 方向固定寫成「Consumer depends on Provider」。

### 2. 子 PRD索引

欄位：

- ID
- 子 PRD
- 完整交付能力
- Spec path
- 是否可作第一個工作

不放 Owner 或進度。

### 3. 依賴類型

- Hard dependency：Provider 未完成指定結果，Consumer 無法開始。
- Contract dependency：介面核准後，Consumer 可用 deterministic adapter 或 fixture 開發；正式整合仍等待 Provider。
- Integration dependency：個別功能可獨立開發，只有聯合驗收需要雙方完成。
- No dependency：可直接開始。

### 4. Dependency edges

建議欄位：

- Edge ID
- Consumer
- Provider
- Type
- Required output／contract
- Start condition
- Integration condition
- If unavailable
- Exception approval

### 5. 全域整合閘門

列出不屬於任何單一子 PRD、但產品完成前必須驗證的項目：

- 所有子 PRD使用相容的 Snapshot、Event、EvaluationResult 與 activation contract。
- 三個 tabs、metrics、Skill Library 與 events 對應同一 Snapshot。
- Live Run 與 Sandbox Run 隔離。
- 正常 workload、Flash Sale、既有 Skill 重用、Candidate 成功及失敗路徑皆通過。
- 90 秒 Demo 流程通過。

### 6. 尚待決定

只放會改變依賴或阻斷關係的問題。一般產品問題應留在總 PRD或相關子 PRD。

## 目前四份候選子 PRD的依賴示例

子 PRD ID：

- SP-01 `scheduling-arena.md`
- SP-02 `existing-skill-adaptation.md`
- SP-03 `candidate-evaluation-loop.md`
- SP-04 `skill-promotion-and-recovery.md`

建議 edges：

- SP-02 consumes SP-01 的 Snapshot、metrics、events 與 policy activation contract。介面核准後可用 adapter 開發；整合驗收等待 SP-01。
- SP-03 consumes SP-02 的 adaptation trigger、既有 Skill 評估結果及 all-skills-failed 判定。介面核准後可用 fixture 開發。
- SP-04 consumes SP-03 的 passed EvaluationResult，亦 consumes SP-01 的 Skill registration、dispatch 與 Snapshot contract。可用固定 passed result 平行準備；正式驗收等待 SP-01 與 SP-03。
- 全域 Demo integration depends on SP-01 至 SP-04 全部完成，但它不是新的子 PRD。

這些是規格依賴候選，正式 edges 須在子 PRD內容完成後再核對。

## final 中確定需要的檔案

### 根目錄

- `README.md`
- `AGENTS.md`
- `INDEXER.md`
- `CONTEXT.md`
- `PRD.md`
- `ACCEPTANCE.md`
- `workflow.md`
- `sub-prd-authoring.md`

### 流程與契約

- `docs/adr/0001-document-authority.md`
- `docs/adr/0002-one-branch-per-sub-prd.md`
- `docs/contracts/backend-contract.md`

### 子 PRD與模板

- `sub-PRDs/DEPENDENCIES.md`
- 四份候選正式子 PRD
- `templates/sub-prd.md`

## 尚未確定清楚，但建議補上的必要模板

### `templates/parent-issue.md`

建議新增。比賽當日由 Owner 依子 PRD建立 GitHub parent issue。模板至少包含：

- 子 PRD ID、路徑及版本。
- Owner。
- 核准紀錄。
- Scope 摘要或連結。
- Dependency readiness。
- Ticket／sub-issue 連結。
- 子 PRD整體驗收與 closeout 狀態。

此模板不保存即時狀態；它只規定 GitHub issue 應如何記錄狀態。

### `templates/ticket.md`

建議新增。即使比賽當日使用 `$to-tickets`，final 仍應自足，不假設 skill 永遠可用。模板至少包含：

- Parent issue。
- What to build：使用者可見的完整路徑。
- Acceptance criteria。
- Blocked by。
- Ready evidence。
- 完成後應留下的 tests、AI review、commit、限制及未決事項。

模板不可預填正式 Ticket，也不可指定檔案路徑或程式碼。

## 尚待設計內容，但不另增檔案

- Requirement coverage：建議放 `ACCEPTANCE.md`，把每項總 PRD要求映射至子 PRD及證據，避免另建第二張需求表。
- 全域未決問題：放總 PRD；子功能問題放相關子 PRD，不另建 `OPEN_QUESTIONS.md`。
- 即時工作狀態：只放 GitHub issues，不建 `STATUS.md`。
- AO 草案：留在 `_2`，final 不建 `tickets/` 或 `drafts/`。
- 來源歷史：由 planning repo 與 discussion records 保存，final 不建 `MIGRATION.md`。

## 建議的 final 目錄更新

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
│   ├── sub-prd.md
│   ├── parent-issue.md
│   └── ticket.md
└── sub-PRDs/
    ├── DEPENDENCIES.md
    ├── scheduling-arena.md
    ├── existing-skill-adaptation.md
    ├── candidate-evaluation-loop.md
    └── skill-promotion-and-recovery.md
```

## 本輪結論

- `DEPENDENCIES.md` 應是靜態依賴索引，不是狀態表。
- 需求覆蓋及完成證據放 `ACCEPTANCE.md`。
- final 尚缺兩個重要模板：GitHub parent issue 與 Ticket。
- 不新增 `STATUS.md`、`OPEN_QUESTIONS.md`、`drafts/`、`tickets/` 或 `MIGRATION.md`。
- final 的檔案集合已接近完整；主要未決事項轉為各檔內容與四份子 PRD的最終邊界驗證。
