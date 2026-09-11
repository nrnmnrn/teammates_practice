# Session 1 提示詞：建立 build-handoff_final

請在本 planning repository 建立 `build-handoff_final/`。整合 `build-handoff_2/` 的最新版題目與正式基準流程。

## 邊界

- 只建立或修改 `build-handoff_final/`。
- 保留 `build-handoff/`、`build-handoff_2/`、`discussion_record/` 及其他既有檔案原狀。
- 不建立競賽程式碼、GitHub Issue、正式 Ticket、commit、push、merge 或 deploy。
- `build-handoff_final/` 通過獨立驗收前，只是候選 handoff。
- 不以目錄完整為由增加無證據文件。

使用 `$caveman:caveman` 的 `wenyan-ultra` 模式回報。建立或修改 Agent 會讀取的文件時，使用 `$writing-for-agents`。持久文件使用清楚、正常的繁體中文，不使用文言縮寫。

## 步驟 1：載入規則

完整閱讀：

1. repository 根目錄及適用範圍內的 `AGENTS.md`。
2. `docs/operations/build-handoff-final-sessions/DECISIONS.md`。
3. `docs/competition/README.md`。
4. `docs/operations/team-readiness.md`。
5. `docs/playbook/workflow.md`。
6. `docs/operations/team-work-allocation.md`。
7. `build-handoff/sub-prd-authoring.md`。
8. `DECISIONS.md` 所列主要討論來源中，與待處理分支有關的完整紀錄。

完成條件：能分別說明產品權威、流程權威、舊 handoff 角色、AO 草案狀態及候選 final 狀態。

## 步驟 2：盤點來源

先用 `rg --files` 建立 `build-handoff/` 與 `build-handoff_2/` 的完整檔案清單。不可只比較同名檔案。

依下列層級讀取，以減少重複 token：

1. 所有檔案先取得路徑、大小、標題、交叉引用及必要 hash。
2. 用關鍵詞及標題判斷用途、權威與可能內容對應。
3. 只有會成為 final 來源、疑似衝突或可能含獨有需求的檔案才完整閱讀。

AO-001 至 AO-010 只需由一位唯讀 worker 檢查是否含總 PRD 未記載的產品需求。不得把 AO 拆解方式帶入 final。

若使用 Sub-agent，依 repository 的 Sub-agent Routing 執行：

- 使用兩位 `gpt-5.6-luna` 唯讀 worker，`fork_turns: "none"`。
- Worker A 只盤點 `build-handoff/`，輸出「直接採用／需改寫／不搬入」及證據。
- Worker B 只盤點 `build-handoff_2/`，輸出產品來源、正式 PRD、contract、AO 獨有需求檢查及證據。
- 每位 worker 回報精簡表格、完整路徑、行號與最小證據，不重述全文。
- 若指定模型不可用，停止並回報，不得替換模型。

完成條件：每個來源檔都有用途分類，且 final 的每個預計檔案都有來源或必要性證據。

## 步驟 3：制定搬移表與子 PRD 邊界

在修改前先於 Session 內形成搬移表：

- final 路徑。
- 來源路徑。
- 採直接保留、局部改寫或重新整合。
- 改寫理由。
- 不採用內容及原因。

驗證候選子 PRD：

- 每份形成輸入到可見結果的完整功能切片。
- 每份能獨立展示、測試及驗收。
- 每份合理預估為 2 至 3 張 vertical-slice Ticket。
- 4 至 5 張須有特別核准欄位；超過 5 張即重新切分或縮減範圍。
- 所有子 PRD 合計覆蓋總 PRD，沒有缺口或重複責任。
- `DEPENDENCIES.md` 能區分可按 contract 平行開發與真正 blocking integration。

完成條件：子 PRD 切法、coverage 及依賴均有來源支持；未決產品問題已標明，沒有靠猜測填滿。

## 步驟 4：建立 final

先由主 Agent 建立必要目錄骨架，再分配互不重疊的檔案集合。每個檔案只能有一位 writer。

可使用兩位 `gpt-5.6-terra` writer，`fork_turns: "none"`：

- Writer A：`README.md`、`AGENTS.md`、`INDEXER.md`、`workflow.md`、`sub-prd-authoring.md`、`docs/adr/`、`templates/`。
- Writer B：`PRD.md`、`ACCEPTANCE.md`、條件式 `CONTEXT.md`、`docs/contracts/`、`sub-PRDs/`。

主 Agent 提供每位 writer：明確檔案範圍、搬移表、必要來源、已確認決策、禁止事項與完成證據。Writer 不得修改未分配檔案。若指定模型不可用，停止並回報，不得替換模型。

優先保留仍正確的既有文字。只有在產品內容、流程規則、路徑、文件權威或自足性需要時才改寫。

`sub-prd-authoring.md` 必須明確落實：2 至 3 張為標準；4 至 5 張須特別核准；超過 5 張不得核准，須拆分或縮減範圍。

完成條件：預定 final 文件已建立；沒有 AO Ticket、預排 Owner、過時題目或尚未核准卻寫成確定要求的內容。

## 步驟 5：主 Agent 整合檢查

主 Agent 檢查並只修正必要的一致性問題：

1. 需求 coverage：`build-handoff_2/PRD.md` 的每項需求都有承接位置。
2. 流程 coverage：兩份正式流程基準的開始條件、工作順序、依賴、驗收、review、commit、merge 與 closeout 都有承接位置。
3. 權威一致：總 PRD、子 PRD、contract、Ticket 及執行狀態各有單一責任。
4. 依賴一致：中央索引與各子 PRD 相符，方向皆為 Consumer depends on Provider。
5. 路徑一致：所有相對連結有效；final 不必回到 planning repository 才能理解或執行。
6. 狀態一致：題目已獲准更換；Owner 與 Ticket 留待比賽當日；final 尚待獨立驗收。
7. 範圍一致：沒有研究全文、會議歷史、個資、秘密、淘汰方案或競賽程式碼。

執行 `git diff --check`。這是純文件工作，不執行 Ruff、Pytest、format、type check 或 build。

## 最終回報

依 repository Completion Report 回報：

- 建立結果。
- 修改檔案。
- 各檔案來源與作用。
- 已由證據解決的問題。
- 仍為「尚待決定」的問題。
- 命令與結果。
- 假設、限制與風險。
- 完整 ELI5 說明。

最後明確聲明：本 Session 沒有判定 final 已可取代 `build-handoff/`；該判定保留給 Session 2。
