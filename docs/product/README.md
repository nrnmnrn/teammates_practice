# 產品入口

唯一自足的實作產品規格是 [build-handoff/PRD.md](../../build-handoff/PRD.md)。它定義目前方向：單節點排程、受限的真 AI 策略提案，以及固定比較驗證。

使用者已確認採中文整合 PRD，且比賽從空專案開始；全員確認、演練與凍結仍待完成。此文件不代表主辦方背書。

## 歷史與比較來源

- [Hackathon_PRD.md](../../Hackathon_PRD.md)：原版排程 UI 規格。
- [hackathon-prd_modified.md](../../hackathon-prd_modified.md)：比較用修改稿。
- [hackathon-prd.md](../../hackathon-prd.md)：舊版英文 MiniGrid PRD。

上述及任何其他舊 PRD／候選稿只供理解背景與比較，不是與 `PRD.md` 並行開發的規格，也不應複製入未來實作 repo。

## 原始報名內容摘要

- 方向：自主且具適應能力的 AI（Autonomous & Adaptive AI）。
- 問題：傳統 RL agent 缺乏跨場景泛化能力，轉移到新環境時常需重構策略與訓練流程。
- 對象：AI 研究人員、虛擬 Agent 開發者與遊戲開發團隊。
- 原提案：在 MiniGrid 結合 Codex 驅動的失敗反思／重規劃，以及通用 RL 策略與任務條件化架構。
- 原成果：展示多環境任務遷移與失敗自癒的 prototype。
- 原指標：成功率、跨環境泛化、樣本效率、災難性遺忘。

## 按需參考

[得獎團隊研究](../research/sea-openai-winning-patterns.md)、[Vibe Coding 操作手冊](../playbook/README.md)、[賽前時程](../operations/schedule.md)可按需要閱讀；它們不取代 PRD。工具準備與團隊演練仍依既有安排進行。

在正式開發前，規劃 repo 不建立可直接搬入決賽的程式碼、RL pipeline、Agent loop 或 reward logic。
