# Agent 與 Demo UI 子 PRD 放置方式討論紀錄

- 日期：2026-09-12
- 性質：對話紀錄，供後續討論追蹤
- 權威限制：本文件不是總 PRD、子 PRD、Ticket、核准紀錄或流程規範。若與正式文件衝突，以正式文件及最新團隊決議為準。

## 延續決策

- `build-handoff_final/` 將整合 `build-handoff/` 的流程與 `build-handoff_2/` 的完整產品內容。
- 賽前只準備總 PRD、正式子 PRD、依賴表及 handoff 流程。
- Owner、GitHub parent issue 與正式 Ticket 留待比賽當日產生。
- AO-001 至 AO-010 暫留 `_2` 作拆解參考，不搬入 final。
- `sub-PRDs/` 預定存放依正式流程製作的子 PRD。

## 本輪由于喬提出的不確定事項

目前不確定應如何處理：

- `build-handoff_2/prd/AGENT_ORCHESTRATION_PRD.md`
- `build-handoff_2/prd/Hackathon_DEMO_UI_PRD.md`

可能作法包括直接放進 `sub-PRDs/`、改放別處，或先放入再於後續拆分。

## 建議結論

`sub-PRDs/` 只放已依正式模板整理、邊界明確的子 PRD。兩份 `_2` 原檔不應原樣搬入 final，也不建議先搬入後再拆。

原檔繼續保留在 `build-handoff_2/prd/` 作來源材料。整合時從其中提取完整產品內容，重製為 final 的正式子 PRD。如此可避免 final 同時保存來源草稿與正式規格。

## Agent Orchestration 判斷

`AGENT_ORCHESTRATION_PRD.md` 已有相對清楚的完整功能邊界：負責 adaptation 狀態、決策、版本迭代、事件與錯誤恢復；Scheduler、UI 與 Sandbox 由其他 adapter 提供。

因此，暫建議重製為：

```text
build-handoff_final/sub-PRDs/agent-orchestration.md
```

目前無須再拆成多份子 PRD。既有 Skill 選擇、Candidate 迭代、Evaluator gate、automatic activation 與錯誤恢復共同形成一個完整 adaptation lifecycle。其內部步驟日後較適合拆成 Ticket，而非賽前拆成更多子 PRD。

只有在整理時發現某一部分可獨立交付、獨立驗收、由另一 Owner 合併，且不依賴同一狀態機，才考慮另拆子 PRD。

## Demo UI 判斷

`Hackathon_DEMO_UI_PRD.md` 同時涵蓋 UI、Simulator、Adapter、Metrics、排程規則、測試與瀏覽器驗收。它可能是一份完整 Demo 功能，也可能混入 Scheduler runtime 的責任。

因此，不應先假定必拆，也不應原樣搬入。應先建立需求覆蓋與責任表，判斷：

- Gradio 頁面、互動、狀態呈現及瀏覽器驗收屬 Demo UI。
- Simulator、排程規則與 policy execution 是否有自己的非 UI 行為及測試。
- Adapter 是 UI 依賴的 contract，還是 UI 子 PRD 的交付內容。
- Metrics 的計算由 Scheduler 產生，還是 UI 計算並呈現。

若 Simulator／Scheduler 可在沒有 UI 的情況下獨立運作及驗收，宜另建 Scheduler 子 PRD，Demo UI 只負責呈現與控制。若它只是 Demo UI 的內建展示資料來源，則可留在 Demo UI 子 PRD。

完成此判斷後，才建立：

```text
build-handoff_final/sub-PRDs/demo-ui.md
```

如需另拆，再建立對應 Scheduler 子 PRD。不得先以檔案長度或技術名稱決定拆分。

## 為何不設 final 內的草稿區

不建議在 final 新增 `drafts/` 或 `source-prds/`：

- final 應可直接交付未來 implementation repo。
- 多一份來源草稿會產生第二套規格。
- Agent 可能誤讀草稿並建立工作。
- 原始資料已完整保存在 `build-handoff_2/`，無須在 final 重複保存。

整合來源與決策脈絡由 planning repo 的 discussion record 保存；final 只保留執行所需文件。

## 正式建立前的判定問題

每份候選子 PRD 必須通過：

1. 是否交付一個完整、可觀察的功能？
2. 是否能獨立驗收成功或失敗？
3. 是否有清楚輸入、輸出及直接依賴？
4. 是否能由一位 Owner 負責整體 closeout？
5. 是否能在一個 branch／Draft PR 內合理合併？
6. 內部剩餘拆解是否只是 Ticket 級工作？

通過後才放入 `sub-PRDs/`。未通過者留在來源文件，繼續整理，不進 final。

## 本輪結論

- Agent Orchestration：重製後放入 `sub-PRDs/`；目前傾向維持一份子 PRD。
- Demo UI：先釐清與 Simulator／Scheduler 的責任邊界；確定後重製並放入 `sub-PRDs/`。
- 兩份 `_2` 原檔：不原樣搬入 final，不另設草稿資料夾。
- 不採「先搬入、以後再拆」；先決定邊界，再讓 final 成為單一可信來源。
