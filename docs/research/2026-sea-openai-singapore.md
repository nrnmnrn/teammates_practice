# Sea × OpenAI 新加坡站：參考先例研究（正式名稱與年份待核對）

> 狀態：參考先例（informative precedent），不是台灣站正式規則或本隊產品需求。
> 建檔日期：2026-09-08。此筆記將既有總覽中的新加坡資料移入；尚未在本輪重新查證所有來源。檔名依目錄規則暫定，正式賽事名稱與年份仍待主辦方原始公告核對。

## 研究範圍

本筆記記錄 Sea × OpenAI Codex Hackathon Singapore 的公開資訊與得獎案例，供未來跨賽事比較。它不推定台灣站採用相同題目、評分方式、時程或提交規則。

## 直接來源

| 類型 | 來源 | 查閱日期 | 用途 |
| --- | --- | --- | --- |
| 主辦方回顧 | [Sea 官方回顧](https://www.sea.com/news/407) | 2026-09-07 | 賽事規模與系列賽背景 |
| 活動頁 | [Singapore 官方活動頁](https://luma.com/kv0kks2a?locale=en-GB) | 2026-09-07 | 題目方向、流程與公開評估面向 |
| 團隊作品 | [Astrail GitHub 專案說明文件（README）](https://github.com/BrownBOBAsushi/Astrail_Hashkey) | 2026-09-07 | TripCanvas 的公開作品做法 |

### 補充來源

下列是既有總覽使用的報導或團隊貼文，可協助理解得獎案例；它們不是主辦方規則的來源，未來若要擴寫事實應優先補上原始作品或主辦方公告。

- [The Business Times 賽事報導](https://www.businesstimes.com.sg/startups-tech/technology/ai-innovation-inaugural-sea-openai-regional-codex-hackathon-singapore)
- [Evoloop 團隊成員說明](https://www.linkedin.com/posts/lakshmi-aishwarya-vathada-756139206_happy-to-announce-that-we-won-the-sea-x-activity-7469560657307140097-zhAw)
- [Shopee Live Producer 團隊成員說明](https://www.linkedin.com/posts/kpriyadharshan_openai-sea-codex-activity-7469403743986298880-SdJB)

## 來源事實

- Sea 的官方回顧指出，該站有超過 1,200 份申請，最終選出 40 隊、144 位參加者，並將系列賽帶往台灣等市場。[Sea 官方回顧](https://www.sea.com/news/407)
- 官方活動頁列出三個方向：Autonomous & Adaptive AI（可自主行動並因應變化的 AI）、AI-Native Products & Operations（以 AI 作為產品或作業核心）、Deep Domain AI（深入特定產業知識的 AI）；並公開 problem framing（問題界定）、build quality（成品品質）、depth of thought（思考深度）與 Codex usage（Codex 使用情況）等評估面向。[Singapore 官方活動頁](https://luma.com/kv0kks2a?locale=en-GB)
- 官方活動頁描述先由各隊向評審簡報，再選出 finalist（進入最後展示階段的隊伍）上台展示。[Singapore 官方活動頁](https://luma.com/kv0kks2a?locale=en-GB)
- Astrail 的公開專案說明文件顯示其將社群旅遊靈感轉為行程規劃，並呈現地圖、來源證據、信心程度、取捨說明、使用者核准與可重播的快取 Demo（展示用的預先準備流程）。[Astrail GitHub 專案說明文件（README）](https://github.com/BrownBOBAsushi/Astrail_Hashkey)

## 研究推論

下列為依公開作品與賽事描述整理的推論，不是主辦方規則：

- 可被使用者檢視的決策依據，能讓 agent（可依資料做判斷或執行任務的 AI 系統）的輸出較容易被覆核。
- 將雜亂輸入轉成有下一步的產出，比單輪聊天更能呈現完整的產品流程。
- 對不確定或高風險情況保留人工確認，可使 AI 在既有工作流程中的角色更清楚。

## 候選參考做法

以下是可供日後黑客松分工與流程規劃參考的研究洞見；它們只描述可再評估的方向，尚未被本隊採納為角色分配、工作流程、時程、台灣站規則或產品要求。

### 讓決策依據可被覆核

- **來源證據：**補充來源的 [Evoloop 團隊成員說明](https://www.linkedin.com/posts/lakshmi-aishwarya-vathada-756139206_happy-to-announce-that-we-won-the-sea-x-activity-7469560657307140097-zhAw) 提及 white-box（可檢查內部決策或程式改變）取向；[The Business Times 賽事報導](https://www.businesstimes.com.sg/startups-tech/technology/ai-innovation-inaugural-sea-openai-regional-codex-hackathon-singapore) 僅可作背景。兩者皆非作品原始資料，**待補原始來源**。
- **研究推論：**若未來題目涉及 agent 輸出或程式變更，保留可檢視的決策依據可能較容易說明輸出來源與人可檢查的部分。
- **何時值得討論：**題目確認後，若需展示 agent 的判斷或程式變更，可討論決策依據、變更摘要或檢查畫面是否有助於說明。
- **未來規劃問題：**需要哪些可檢視證據？哪些內容適合向使用者或評審呈現？
- **限制／風險：**目前依團隊貼文與報導，未證實完整做法；也不表示每個題目都需要或適合揭露相同程度的內部資訊。
- **待核實項目：**作品 repository（程式專案存放庫）、demo（展示）或主辦方資料，以及 white-box 做法的具體範圍。

### 將雜亂輸入整理成有下一步的產出

- **來源證據：**直接來源的 [Astrail GitHub 專案說明文件（README）](https://github.com/BrownBOBAsushi/Astrail_Hashkey) 顯示 TripCanvas 呈現來源證據、取捨、使用者核准與可重播的快取 Demo。
- **研究推論：**這個案例顯示，未來展示可由「能回答問題」進一步思考為「把輸入整理成有下一步的產出」的完整使用情境。
- **何時值得討論：**題目確認後，若輸入資料雜亂且使用者需要採取下一步行動，可討論輸入、產出、確認點與展示路徑的界定。
- **未來規劃問題：**哪些輸入需要整理？產出要支持哪一個下一步？是否需要準備可重播材料？
- **限制／風險：**這是旅遊場景案例，不等同於本隊未來題目、資料條件或可用資源；README 也無法單獨證明完整實作與評估效果。
- **待核實項目：**公開 README 所列做法是否反映完整作品，以及其展示以外的實作與評估資料。

### 對不確定性保留確認與升級途徑

- **來源證據：**補充來源的 [Shopee Live Producer 團隊成員說明](https://www.linkedin.com/posts/kpriyadharshan_openai-sea-codex-activity-7469403743986298880-SdJB) 與 [The Business Times 賽事報導](https://www.businesstimes.com.sg/startups-tech/technology/ai-innovation-inaugural-sea-openai-regional-codex-hackathon-singapore) 描述分流、grounded（根據已提供資料的）回覆與升級案件；兩者皆非完整原始作品資料，**待補原始來源**。
- **研究推論：**若未來需處理 AI 難以可靠判斷的情況，保留確認與人工處理的界線可能有助於說明系統如何處理不確定性。
- **何時值得討論：**題目確認後，若存在高不確定性或需要人工判斷的輸入，可討論確認點、人工處理範圍及其展示方式。
- **未來規劃問題：**哪些輸入需要確認？哪些情況可交由人工處理？如何在 demo 中呈現這些邊界？
- **限制／風險：**尚未取得完整原始作品資料，不能據此推定技術細節、效能或適合的處理方式；也不可據此預設台灣站會評估此類設計。
- **待核實項目：**原始作品資料、分流與升級的具體做法，以及其適用條件。

## 限制與本隊差異

- 這是單一賽事的前例，尚不足以形成跨賽事共通模式。
- 新加坡的題目、評分與流程不能視為台灣站規則；台灣站資訊以 [competition README](../competition/README.md) 的官方與隊內確認資料為準。
- 本隊產品決策仍等待主辦方對題目變更的回覆；本筆記不應被當成需求文件或交接內容。

## 未決問題

- 補齊各得獎作品的原始程式專案存放庫（repository）、展示資料（demo）或主辦方得獎公告，以降低對二手報導的依賴。
- 核對賽事正式名稱、年份與得獎名次的主辦方原始公告；既有補充報導將冠軍列為 Team Untitled.ai 的 Evoloop、亞軍列為 Team TripCanvas、季軍列為 Team Techbros 的 Shopee Live Producer，但在取得直接來源前不視為已確認事實。
- 完成另一場不同賽事的研究筆記後，才評估是否存在有來源支持的共通模式。
