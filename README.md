# SEA × OpenAI Hackathon 規劃工作台

這個 repository（repo）供團隊在決賽前整理規則、研究、操作手冊與交接資料。它不是 9 月 12 日比賽當天的程式 repo。

## 現在先做什麼

1. 全員先讀 [比賽資訊](docs/competition/README.md)，確認哪些內容已證實、哪些仍待主辦方回覆。
2. 閱讀 [得獎團隊研究](docs/research/sea-openai-winning-patterns.md)，把可借鏡之處與正式需求分開。
3. 閱讀唯一實作產品規格：[build-handoff/PRD.md](build-handoff/PRD.md)。使用者已確認中文整合 PRD、比賽從空專案開始的方向；尚待全員確認、演練及凍結，且不代表主辦方背書。
4. 依 [Vibe Coding 操作手冊](docs/playbook/README.md)完成安裝與練習，並依 [賽前時程](docs/operations/schedule.md)與[團隊準備狀態](docs/operations/team-readiness.md)完成既有演練安排；產品行為以 PRD 為準。

Repo owner 可從 [Owner backlog](TODO.md)管理尚待補齊的文件與研究；它不取代團隊的賽前操作安排。

## Repo 分工

- **規劃 repo（本 repo）**：保存完整比賽資訊、研究、教學、會議紀錄與產品規格。
- **實作 repo**：9/12 開賽後建立，只接收凍結後的精簡交接包及當天產生的程式碼。
- **實作交接包**：兩個 repo 之間唯一的文件介面。研究筆記、會議紀錄與長篇教學不會複製過去。

## 文件地圖

| 路徑 | 用途 | 目前狀態 |
|---|---|---|
| [`CONTEXT.md`](CONTEXT.md) | 團隊共用名詞 | 可使用 |
| [`docs/competition/`](docs/competition/README.md) | 規則、時程、資源及待確認事項 | 初版 |
| [`docs/research/`](docs/research/2026-09-08-scheduler-ui-tools.md) | 歷屆案例、工具研究與可轉用策略 | 初版 |
| [`docs/playbook/`](docs/playbook/README.md) | 新手 Vibe Coding 手冊 | 初版 |
| [`TODO.md`](TODO.md) | owner 的文件與研究待辦 | 進行中 |
| [`docs/operations/`](docs/operations/schedule.md) | 會議及演練安排 | 初版 |
| [`docs/product/`](docs/product/README.md) | 產品方向、歷史／比較來源與規格入口 | 使用者確認方向，待團隊確認 |
| [`build-handoff/`](build-handoff/README.md) | 比賽實作 repo 的最小輸入與唯一 PRD | PRD 待團隊確認與演練 |
| [`docs/adr/`](docs/adr/0001-separate-planning-and-build-repositories.md) | 難以回頭的重要決策 | 可使用 |

## 維護原則

- 繁中為主，必要的產品與技術名詞保留英文。
- 事實需附來源；新加坡站資訊只作參考，不冒充台灣站正式規則。
- 比賽規則、研究心得、產品需求分開存放，避免推測變成需求。
- 文件 owner 為林于喬；重要操作文件必須由第一次使用者實際驗證。
- 不在 repo 中存放 API key、SSH private key、密碼或其他 secrets。
