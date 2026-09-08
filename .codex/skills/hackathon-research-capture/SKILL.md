---
name: hackathon-research-capture
description: "Capture evidence-backed hackathon strategy, participation-flow, and team-collaboration cases or topic batches as bounded candidates. Use for this planning repo's hackathon research, not general research or product implementation."
---

# Hackathon Research Capture

將本 repo 的黑客松策略、參賽流程與團隊協作研究，整理成可追溯、可比較的候選做法；不把其他賽事誤寫為 Sea × OpenAI 台灣站規則，也不直接產生產品或實作決策。

先讀取 repo 的 `AGENTS.md` 與 [研究記錄規格](references/research-record.md)。僅在需要發現或查證外部事實時使用 `$research`；若只是整理既有資料、更新研究總覽索引或評估候選，直接處理，不調用 `$research`。

## 路由

- **單案例研究**：使用者指定某篇文章、團隊或作品時採用。一次 `$research` 只處理一個案例，並產生一份案例檔；主 agent 隨後覆核來源、分類、台灣站誤套風險與跨案例門檻，再更新索引。
- **主題批次研究**：使用者指定某種做法、限制或參賽經驗時採用。先讀取 [多案例研究指南](references/ai-agent-multi-case-session.md)，將寬題縮成一個可比較的子題。批次是協調層：每個入選案例仍各自執行一次 `$research`、各有一份案例檔；只有主 agent 可在所有案例覆核後撰寫主題摘要與更新索引。不得把多個案例混入同一次 `$research` 或同一份案例檔。
- 題目不夠明確而無法選定單案例或單一子題時，先說明理解並請使用者決定；題目明確時可直接執行。
- 既有研究：依 reference 評估證據品質、補齊「待核實項目」，或判定是否能從案例觀察升為跨案例候選。
- 升級決策：研究只能提出候選。寫入 `team-playbook` 或 `build-handoff` 前，依 reference 的明確採納與範圍邊界處理；在主辦方回覆換題問題前，產品／實作決策維持候選或暫緩。

不要保存來源全文、大型資料、多餘個資、私人通信或任何秘密。沒有資料時明確保留空缺，不得補造。
