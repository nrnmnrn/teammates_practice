---
name: hackathon-research-capture
description: "Capture evidence-backed hackathon strategy, participation-flow, and team-collaboration research as bounded candidates. Use for this planning repo's hackathon cases, not general research or product implementation."
---

# Hackathon Research Capture

將本 repo 的黑客松策略、參賽流程與團隊協作研究，整理成可追溯、可比較的候選做法；不把其他賽事誤寫為 Sea × OpenAI 台灣站規則，也不直接產生產品或實作決策。

先讀取 repo 的 `AGENTS.md` 與 [研究記錄規格](references/research-record.md)。僅在需要發現或查證外部事實時使用 `$research`；若只是整理既有資料、更新研究總覽索引或評估候選，直接處理，不調用 `$research`。

## 路由

- 新研究：一次 `$research` 只處理一個案例，並產生一份案例檔；主 agent 隨後覆核來源、分類、台灣站誤套風險與跨案例門檻，再更新索引。
- 既有研究：依 reference 評估證據品質、補齊「待核實項目」，或判定是否能從案例觀察升為跨案例候選。
- 升級決策：研究只能提出候選。寫入 `team-playbook` 或 `build-handoff` 前，依 reference 的明確採納與範圍邊界處理；在主辦方回覆換題問題前，產品／實作決策維持候選或暫緩。

不要保存來源全文、大型資料、多餘個資、私人通信或任何秘密。沒有資料時明確保留空缺，不得補造。
