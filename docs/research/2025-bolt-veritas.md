# 2025 World’s Largest Hackathon presented by Bolt：Veritas（線上）

研究日期：2026-09-09
資訊分類：賽事與提交關係、公開作品證據為「已確認資訊」；工具流程、時間、故障與整合為參賽者第一手自述；本案所有可借鏡內容皆是「參考性先例」，不是 Sea × OpenAI 台灣站規則或本隊決定。

## 研究問題

Veritas 是否符合 `2026-09-multi-person-ai-coding-alignment` 的單案例納入條件：至少兩名可核實參與者、可檢查作品，以及第一手資料能追溯多人 AI coding、時間節奏、整合或返工／故障？

## 基本資料

| 欄位 | 可直接查驗的內容 |
| --- | --- |
| 賽事 | World’s Largest Hackathon presented by Bolt；官方 Devpost 賽事頁列為線上、公開活動，由 StackBlitz／Bolt 主辦、Devpost 管理。[賽事頁](https://worldslargesthackathon.devpost.com/) |
| 官方活動時段 | Devpost 賽事頁列 2025-05-30 至 2025-06-30；規則要求新 application 主要以 Bolt.new 建立。[賽事頁](https://worldslargesthackathon.devpost.com/) [規則](https://worldslargesthackathon.devpost.com/rules) |
| 研究對象 | Veritas：交易行為分析與 AI coaching app。內容、工具、故障與成果主要是團隊在提交頁的自述，未作獨立程式或安全稽核。[作品頁](https://devpost.com/software/veritas-4lmbop) |
| 提交者 | Devpost 的 Created by 顯示 Kalisetti Nihanth Naidu、Shashank Alla、Shashank Alla；後兩列同名，不能據此推定有三名不同的人。[作品頁](https://devpost.com/software/veritas-4lmbop) |
| 公開作品 | 提交頁連到 `lighthearted-cheesecake-82d5da.netlify.app`；研究日以 HTTP 200 取得頁面，頁面標題為「Veritas - Elite Trading Analysis」。[作品頁](https://devpost.com/software/veritas-4lmbop) [live demo](https://lighthearted-cheesecake-82d5da.netlify.app/) |

## 狀態

**案例觀察；符合本主題批次的單案例納入條件。** 這不是得獎案例；本次查證沒有找到主辦方把 Veritas 列為得獎者的直接來源。單一案例不構成跨案例候選，也不代表團隊已採納任何做法。

## 來源

### 官方／提交來源

- [World’s Largest Hackathon presented by Bolt 官方 Devpost 賽事頁](https://worldslargesthackathon.devpost.com/)：賽事名稱、線上日期、主辦／管理資訊與提交要求；作者：StackBlitz／Bolt、Devpost；查閱日 2026-09-09。
- [官方 Devpost 規則](https://worldslargesthackathon.devpost.com/rules)：新作品、Bolt.new、公開 demo video 與公開可用 URL 等規則；查閱日 2026-09-09。
- [Veritas Devpost 提交頁](https://devpost.com/software/veritas-4lmbop)：提交與賽事關係、creator 名單、工具流程、作品說明、故障自述、live URL、作者更新；作者／團隊：Kalisetti Nihanth Naidu、Shashank Alla；查閱日 2026-09-09。

### 可檢查作品證據

- [Veritas live demo](https://lighthearted-cheesecake-82d5da.netlify.app/)：由提交頁連出的公開部署；研究日 HTTP 200，僅確認頁面可取得，不等於每項功能或後端服務仍正常。
- [Veritas demo video：Bolt.new hackathon submission](https://www.youtube.com/watch?v=x3GgwzHvipo)：YouTube 頁顯示影片標題、作者 `Veritas12`、長度 7:52；影片描述以第一人稱稱「me and my friend」，並說明重新錄製／及時提交。Veritas 提交頁的 Shashank Alla 更新直接連到此影片。[作品頁更新](https://devpost.com/software/veritas-4lmbop)

### 團隊成員第一手紀錄／補充

- [Shashank Alla 在 Veritas 提交頁的兩則更新](https://devpost.com/software/veritas-4lmbop)：2025-07-01 兩次更新，說明原 YouTube 影片被下架，改連另一個影片，再更新至目前影片；這能直接把影片提交行為連到一名 creator，但沒有提供每人技術責任表。
- [Kalisetti Nihanth Naidu 在同一提交頁建立作品](https://devpost.com/software/veritas-4lmbop)：頁面記錄其於 2025-06-30 建立 project。作品頁的「How we built it」與「Challenges we ran into」是團隊第一人稱自述，但沒有逐句標註由哪名成員完成哪個階段。

## 已確認資訊

- 官方 Devpost 賽事頁確認 World’s Largest Hackathon presented by Bolt 為 2025-05-30 至 2025-06-30 的線上公開活動；官方規則要求新 application 主要以 Bolt.new 建立，並提交公開 demo video 與公開可用作品 URL。這些只適用於該賽事。[賽事頁](https://worldslargesthackathon.devpost.com/) [規則](https://worldslargesthackathon.devpost.com/rules)
- Veritas 的 Devpost 提交頁明確將作品提交到 World’s Largest Hackathon presented by Bolt，並列出至少兩個不同姓名的 creator：Kalisetti Nihanth Naidu、Shashank Alla；同頁另重複列出一次 Shashank Alla，第三列的獨立身分未知。[作品頁](https://devpost.com/software/veritas-4lmbop)
- 提交頁列出的開發流程是 **Bolt.new（Phase 1）→ Cursor（backend／API integration，並使用 Claude 做分析）→ Bolt.new（Phase 2，Supabase／Veritas Chronicle）**。這是作者對自己團隊工作的第一手描述，不是主辦方驗證的工具遙測紀錄。[作品頁](https://devpost.com/software/veritas-4lmbop)
- 提交頁稱作品在「intense 12-day hackathon timeframe」內完成，並說明從 6 月 18 日開始；官方活動窗口其實是 5 月 30 日至 6 月 30 日。故 12 日只能理解為團隊自述的實際開發窗口，不能改寫成賽事官方總時長；也沒有公開總工時或連續工作時數。[作品頁](https://devpost.com/software/veritas-4lmbop) [賽事頁](https://worldslargesthackathon.devpost.com/)
- 提交頁自述遇到 Bolt.new local server 不穩定：本地 server 曾在接近提交時意外無法啟動，團隊稱透過除錯找到 configuration conflicts 並修復，之後完成部署。這是團隊報告的環境故障與處理，不是本研究重現的故障。[作品頁](https://devpost.com/software/veritas-4lmbop)
- 提交頁的作品敘述稱最後版本把 Bolt.new frontend、Cursor／外部 backend、Supabase database 整合為可工作的 app；live URL 在研究日可取得，影片也可開啟。這能支持「有整合交付證據」，不能證明所有 API、database、付費流程或交易分析仍可用。[作品頁](https://devpost.com/software/veritas-4lmbop) [live demo](https://lighthearted-cheesecake-82d5da.netlify.app/) [demo video](https://www.youtube.com/watch?v=x3GgwzHvipo)
- 團隊另自述因成員未滿 18 歲及 international co-founder 的年齡／居住限制，未能在活動期間完成 Stripe payment integration；這是成員自述的產品限制，非官方資格或規則。[作品頁](https://devpost.com/software/veritas-4lmbop)

## 參考性先例

### 關鍵決策、限制、取捨與結果

**來源直接陳述：** 團隊先以 Bolt.new 建立 frontend 與 core flow，再在複雜 API choreography／server-side logic 階段使用 Cursor，之後回到 Bolt.new 實作 Supabase 與 Veritas Chronicle；團隊也稱 local server instability 曾威脅提交，最後以 configuration debugging 修復。12 日是團隊自報的開發時段，且從 6 月 18 日開始。[作品頁](https://devpost.com/software/veritas-4lmbop)

**研究推論：** 這提供「按技術階段更換 AI coding tool，最後再整合與驗證」的可比較觀察。它也顯示環境修復可能成為交付責任的一部分。公開資料沒有說明誰作方向裁決、誰持有 branch／檔案、誰負責 API／database 驗收、是否有 merge conflict，故不能把這段流程推成明確的角色分工制度。

### 候選實務：以階段交接維持共同產品意圖

- **來源證據：** 團隊公開描述了 frontend／core flow、backend／API、database／core functionality 的先後階段，以及 Bolt.new → Cursor → Bolt.new 的工具切換；最後仍需整合成同一個 app。[作品頁](https://devpost.com/software/veritas-4lmbop)
- **研究推論：** 多人或多工具工作流可把「目前完成哪一層、下一階段要保留哪些介面」當成交接單位；本案只證明團隊採用了階段式描述，不證明它有正式 handoff 文件或因此提升品質。
- **何時值得討論：** 團隊要並行使用不同 AI coding tool，且 frontend、backend、database 會在同一 demo 中互相依賴時。
- **未來規劃問題：** 每階段的 owner、輸入／輸出契約、驗收方式與回滾點由誰寫下並裁決？
- **限制／風險：** 本案缺少 prompt、commit／branch 紀錄、同步頻率、merge conflict 或返工清單；不能量化工具切換的成本或成效。
- **待核實項目：** 是否真的有多人並行編輯、如何合併產出，以及 Cursor 與 Bolt.new 之間是否發生語意或程式衝突，公開來源未說明。

### 候選實務：把環境故障列入最後交付檢查

- **來源證據：** 團隊自述 local server 曾無法啟動，並因 configuration conflicts 進行除錯，最後修復、部署。[作品頁](https://devpost.com/software/veritas-4lmbop)
- **研究推論：** AI coding 的快速產碼不會取代 runtime／deployment smoke test；在接近提交時仍需有人負責判定「能否啟動、能否展示、是否可回退」。這是候選做法，不是本隊流程。
- **何時值得討論：** 作品依賴本地 server、外部 API、database 或 deployment platform，且 demo 失敗會影響提交時。
- **未來規劃問題：** 最晚何時凍結功能？誰執行乾淨環境啟動、公開 URL、核心流程與替代 demo 的檢查？
- **限制／風險：** 本案只有團隊敘述，沒有錯誤 log、修復 diff、重現步驟或獨立驗證；不能知道修復是否完整或是否造成其他回歸。
- **待核實項目：** 影片與 live app 是否展示與修復前後相同的版本，及 server／Supabase／API 在提交時的實際狀態，未公開。

## 不可套用台灣站

- 2025-05-30 至 2025-06-30 的賽事窗口、主要使用 Bolt.new、約三分鐘影片、公開 URL／badge、年齡與地區資格，都是 World’s Largest Hackathon presented by Bolt 的資訊；不得當成 Sea × OpenAI 台灣站規則。[賽事頁](https://worldslargesthackathon.devpost.com/) [規則](https://worldslargesthackathon.devpost.com/rules)
- Veritas 的 12 日開發窗口不是台灣站時長，也不是普遍的黑客松時間管理證據；不能外推每個團隊需工作 12 日或同樣的每日工時。
- Bolt.new → Cursor → Bolt.new 的具體順序、Claude／Supabase／Netlify 等工具，及本案的年齡／付款限制，不是本隊工具清單、角色分工、產品需求或授權。
- 「多人」只能確認至少兩個不同姓名被列為 creator；不能據此宣稱固定兩人制、第三名成員存在、各人技術責任，或有正式同步／合併流程。

## 待確認問題

- Devpost creator 清單重複 `Shashank Alla`；第三列是重複資料、同名另一人或平台顯示問題，沒有直接來源可判定。
- 公開自述沒有列出每位成員負責的 feature、branch／檔案、方向裁決、整合、驗證或 demo；責任證據目前只能確認「Kalisetti 建立 project、Shashank 發布影片更新」，不能重建技術責任圖。[作品頁](https://devpost.com/software/veritas-4lmbop)
- 「12 日」沒有每日起訖、總工時、休息時間或是否持續開發資料；只能與官方一個月活動窗口並列，不可換算成精確實際工作時長。[作品頁](https://devpost.com/software/veritas-4lmbop) [賽事頁](https://worldslargesthackathon.devpost.com/)
- 沒有公開 repository、commit history、prompt、AI 對話、測試報告、deployment log 或 merge／rollback 紀錄；不能確認多人交接是否造成語意偏移、程式衝突或返工。
- 研究日可取得 live demo 根頁且 YouTube 影片可播放，但未登入、上傳交易資料、呼叫 API、測試 Supabase、付款流程或驗證 AI 分析正確性；不對產品安全、金融建議或 production readiness 作保證。[live demo](https://lighthearted-cheesecake-82d5da.netlify.app/) [demo video](https://www.youtube.com/watch?v=x3GgwzHvipo)
- 沒有找到主辦方把 Veritas 列為得獎／入選的直接公告；本案只作多人 AI coding 過程的參考案例，不作得獎模式案例。

## 主題納入判定

**符合 `2026-09-multi-person-ai-coding-alignment.md` 的納入條件。**

1. **官方／提交關係：** Veritas Devpost 頁明確列於 World’s Largest Hackathon presented by Bolt；官方賽事頁與規則可確認活動及提交要求。[作品頁](https://devpost.com/software/veritas-4lmbop) [賽事頁](https://worldslargesthackathon.devpost.com/)
2. **多人關係：** 至少兩個不同姓名（Kalisetti Nihanth Naidu、Shashank Alla）被列為 creator；重複的 Shashank 列不能用來增加人數。[作品頁](https://devpost.com/software/veritas-4lmbop)
3. **可檢查作品：** 提交頁、研究日 HTTP 200 的 live demo、可播放的 YouTube demo，三者相互連結。[作品頁](https://devpost.com/software/veritas-4lmbop) [live demo](https://lighthearted-cheesecake-82d5da.netlify.app/) [demo video](https://www.youtube.com/watch?v=x3GgwzHvipo)
4. **第一手過程至少涵蓋兩項：** 團隊自述 AI coding 的三階段工具流程、12 日開發窗口、frontend／backend／database 整合，以及 local server configuration failure 的修復；Shashank 的提交頁更新又直接連到影片。[作品頁](https://devpost.com/software/veritas-4lmbop) [demo video](https://www.youtube.com/watch?v=x3GgwzHvipo)

本案可進入候選案例池，但目前只有「案例觀察」狀態；在另一個可比較案例支持同一候選做法前，不得升級為跨案例候選，也未觸及 `team-playbook/`、`build-handoff/` 或 `TODO.md`。
