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

| 研究洞見 | 公開觀察或來源 | 可帶來的幫助 | 可供未來規劃參考面向 | 適用限制 |
| --- | --- | --- | --- | --- |
| 讓決策依據可被覆核 | 補充來源所稱的 Evoloop 團隊貼文提及 white-box（可檢查內部決策或程式改變）取向；相關賽事報導可作為背景，但尚非作品原始資料。 | 未來若題目涉及 agent 輸出或程式變更，可較容易說明輸出從何而來，以及哪些部分可由人檢查。 | 題目確認後，可研究是否需要準備決策依據、變更摘要或檢查畫面等展示材料，以及如何保留可供檢視的證據。 | 目前主要依團隊貼文與報導，須補上作品 repository（程式專案存放庫）、demo（展示）或主辦方資料；也不表示每個題目都需要或適合揭露同樣程度的內部資訊。 |
| 將雜亂輸入整理成有下一步的產出 | TripCanvas／Astrail 的公開 README 顯示來源證據、取捨、使用者核准與可重播的快取 Demo。 | 可協助未來從「能回答問題」的展示，思考成「能把輸入整理為下一步」的完整使用情境。 | 題目確認後，可參考如何界定輸入、產出、使用者確認點與展示路徑，並判斷哪些部分需要事先準備可重播材料。 | 這是旅遊場景的公開案例，不等同於本隊未來題目、資料條件或可用資源；README 也無法單獨證明完整實作與評估效果。 |
| 對不確定性保留確認與升級途徑 | 補充來源所稱的 Shopee Live Producer 報導與團隊貼文描述分流、grounded（根據已提供資料的）回覆與升級案件。 | 可提醒未來規劃時辨識 AI 難以可靠判斷的情況，讓展示能說明系統在不確定時如何處理。 | 題目確認後，可研究哪些輸入需要確認、哪些情況可交由人工處理，以及如何在 demo 中呈現這些邊界。 | 尚未取得完整原始作品資料，不能據此推定技術細節、效能或適合的處理方式；也不可據此預設台灣站會評估此類設計。 |

## 限制與本隊差異

- 這是單一賽事的前例，尚不足以形成跨賽事共通模式。
- 新加坡的題目、評分與流程不能視為台灣站規則；台灣站資訊以 [competition README](../competition/README.md) 的官方與隊內確認資料為準。
- 本隊產品決策仍等待主辦方對題目變更的回覆；本筆記不應被當成需求文件或交接內容。

## 未決問題

- 補齊各得獎作品的原始程式專案存放庫（repository）、展示資料（demo）或主辦方得獎公告，以降低對二手報導的依賴。
- 核對賽事正式名稱、年份與得獎名次的主辦方原始公告；既有補充報導將冠軍列為 Team Untitled.ai 的 Evoloop、亞軍列為 Team TripCanvas、季軍列為 Team Techbros 的 Shopee Live Producer，但在取得直接來源前不視為已確認事實。
- 完成另一場不同賽事的研究筆記後，才評估是否存在有來源支持的共通模式。
