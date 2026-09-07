# Sea × OpenAI：勝出作品模式與決賽準備研究

> 最後整理：2026-09-07。本文把可公開核對的資料分成兩類：台灣站的「已確認資訊」，以及來自第一站新加坡賽事的「參考先例」。後者有助於準備，但不是台灣站已公布的正式規則。

## 一、台灣站：已確認的官方／主辦公告資訊

- 決賽在 **2026 年 9 月 12 日（六）09:30–21:30** 於 **TICC 台北國際會議中心 4 樓鳳凰廳** 舉行；參賽者須自行攜帶筆電與開發工具。[國立臺灣海洋大學轉載的主辦公告](https://cse.ntou.edu.tw/p/406-1063-128010%2Cr1034.php?Lang=zh-tw)
- 入選團隊須在活動當天使用 **OpenAI Codex** 開發，並在期限內完成產品與成果展示；活動目標是把創意快速做成具實際應用價值的 AI 解決方案。[主辦公告](https://cse.ntou.edu.tw/p/406-1063-128010%2Cr1034.php?Lang=zh-tw)
- 台灣站為 Sea 與 OpenAI 共同主辦的亞太系列賽；公開公告說明其面向熟悉 AI 程式開發、能打造可實際應用產品的開發者。[主辦公告](https://cse.ntou.edu.tw/p/406-1063-128010%2Cr1034.php?Lang=zh-tw)

### 對本隊的直接含意

我們可以在賽前準備問題、使用者流程、資料來源、畫面草圖、Demo 劇本與分工，但不應把既有專案或預先寫好的程式當作活動當日的提交品。新加坡官方活動頁明確規定所有開發須在活動當天進行；台灣站公開公告未另行公布不同規則，因此此處應以主辦方當日指示為準。[新加坡官方活動頁](https://luma.com/kv0kks2a?locale=en-GB)

## 二、新加坡站：可參考、但非台灣站正式規則的先例

新加坡是此系列第一站。Sea 的官方回顧指出，該站有 1,200 多份申請，最終選出 40 隊、144 位參加者；後續系列將前往台灣等市場。[Sea 官方回顧](https://www.sea.com/news/407)

### 題目方向

新加坡官方活動頁列出三個方向：

1. **Autonomous & Adaptive AI**：可在不需要持續人工監督下，仍能可靠因應真實世界變化的 agent。
2. **AI-Native Products & Operations**：AI 是產品或工作流程的核心，而非附加功能。
3. **Deep Domain AI**：深入特定產業實務與具體需求的 AI 解決方案。

以上三項來自[新加坡官方活動頁](https://luma.com/kv0kks2a?locale=en-GB)。台灣公開公告尚未公布完全相同的題目或評分表，故不可視為台灣站保證採用的規則。

### 評審流程與可能的關注點

新加坡站先由每隊輪流向評審簡報，接著選出 finalist 上台展示；官方頁列出的評估面向是問題定義、成品品質、思考深度，以及 Codex 在製作過程中的有效運用。[新加坡官方活動頁](https://luma.com/kv0kks2a?locale=en-GB)

賽後報導則進一步列出當站評分面向：問題定義、成品品質、洞見與原創性、真實世界價值、與三大方向的對齊、以及 Codex 的有效使用；評審也檢視延遲（latency）、資料隱私與賽後發展性。[Business Times 賽事報導](https://www.businesstimes.com.sg/startups-tech/technology/ai-innovation-inaugural-sea-openai-regional-codex-hackathon-singapore)

**準備原則：** 將上述視為高價值的 rehearsal checklist，而不是台灣站正式計分表。真正的規格、時間限制與提交方式，以現場主辦方指示為準。

## 三、歷屆勝出模式，轉化為本隊可執行的準備

### 模式 A：可看見的 agent 決策與進步

**觀察（observation）**：新加坡冠軍 Team Untitled.ai 的 Evoloop 讓遊戲 AI 邊玩邊適應；它的差異點不只是自我演化，還能以 white-box 方式讓人檢視程式如何改變，並提出可延伸到金融等重視可解釋性的場景。[賽事報導](https://www.businesstimes.com.sg/startups-tech/technology/ai-innovation-inaugural-sea-openai-regional-codex-hackathon-singapore)；[團隊成員的獲獎說明](https://www.linkedin.com/posts/lakshmi-aishwarya-vathada-756139206_happy-to-announce-that-we-won-the-sea-x-activity-7469560657307140097-zhAw)

**對本隊的關聯（relevance）**：若我們的產品有 agent，評審不容易只因「它會回答」而相信它有價值；需要看得到它根據什麼資訊行動、遇到不確定時如何停下來，以及輸出為何可信。

**具體行動（concrete action）**：在 MVP 中只挑一條核心任務鏈路，顯示 `輸入 → 檢索／判斷 → 建議或動作 → 使用者確認 → 結果`。在 UI 顯示資料來源、信心程度、關鍵取捨與失敗／轉交人工的狀態；不展示模型的隱藏 chain-of-thought。

**證據／檢查（evidence/check）**：Demo 前用一個正常案例與一個資料不足案例各跑一次。每次都能在 60 秒內指出「agent 用了什麼資料、做了什麼、為何可被使用者覆核」；資料不足時不應捏造答案，而應要求補充或轉人工。

### 模式 B：從雜亂輸入到有邊界的真實任務

**觀察（observation）**：亞軍 Team TripCanvas 把 Instagram Reels 或 TikTok 旅遊靈感轉成行程、航班與旅館建議，甚至做到預訂與付款流程。[賽事報導](https://www.businesstimes.com.sg/startups-tech/technology/ai-innovation-inaugural-sea-openai-regional-codex-hackathon-singapore) 其公開專案文件還呈現地圖、來源證據、信心程度、取捨說明、使用者核准，以及可重播的快取 Demo。[Astrail GitHub README](https://github.com/BrownBOBAsushi/Astrail_Hashkey)

**對本隊的關聯（relevance）**：好的 AI-native 產品不是把聊天框貼到既有流程，而是把原本混亂、耗時的輸入，變成使用者可檢查且可採取下一步的結果。同時，任何有成本或風險的動作都要有清楚邊界。

**具體行動（concrete action）**：把產品入口設計成一個真實但混亂的輸入（例如需求、文件、對話或圖片），在 Demo 中轉為一份可操作產出。若產品會送出、修改、購買或分享資料，先讓使用者在畫面上確認範圍、對象與結果；沒有真實串接時，明確標示為 mock／sandbox。

**證據／檢查（evidence/check）**：Demo 劇本需包含一個可見的「使用者確認」節點，且畫面清楚列出將執行的動作及限制。準備固定測試資料與 fallback／快取資料；斷網或外部 API 失敗時，仍能完整展示產品價值，並誠實標示資料來源狀態。

### 模式 C：在真實工作流程中，做有克制的 AI 協作

**觀察（observation）**：季軍 Team Techbros 的 Shopee Live Producer 不是取代直播主，而是在幕後分流買家問題、生成 grounded 回覆、升級不確定案件、提示高風險說法，並記住直播主已核准的回答。[賽事報導](https://www.businesstimes.com.sg/startups-tech/technology/ai-innovation-inaugural-sea-openai-regional-codex-hackathon-singapore) 團隊成員指出，關鍵是讓 AI 在有脈絡、克制且可信任的條件下行動。[團隊成員貼文](https://www.linkedin.com/posts/kpriyadharshan_openai-sea-codex-activity-7469403743986298880-SdJB)

**對本隊的關聯（relevance）**：若我們選擇一個職場或生活中的工作流程，應明確定義 AI 的角色：哪些事可自動做、哪些只能建議、何時必須交給人。這比泛用聊天機器人更能顯示深度與真實價值。

**具體行動（concrete action）**：為核心流程建立三段式規則：`可自動處理`、`需要使用者核准`、`必須升級／拒絕`。將規則做成可看見的介面狀態，而非只放在簡報文字；只針對一個明確使用者與一種高頻痛點做深。

**證據／檢查（evidence/check）**：準備至少三筆輸入：可安全完成、資料不足、風險過高。Demo 時三筆都必須導向不同且合理的狀態；簡報能用一句話說清楚「為何這一筆不能自動做」。

## 四、決賽當天最小驗收清單

- **問題**：30 秒說出使用者是誰、何時遇到什麼高頻痛點，以及現有做法為何不夠好。
- **產品**：有一條從真實輸入到可驗證結果的完整流程，不只是一張 UI 或單輪聊天。
- **AI 與 Codex**：說得出 Codex 實際協助開發／迭代了哪些部分；若現場規則要求，保留可展示的開發過程或提交紀錄。
- **可信任性**：顯示來源、限制、錯誤處理與人類確認點；不把 mock 當成真實服務。
- **工程品質**：Demo 走過一次離線／外部服務失敗的備援流程；避免把成敗押在不穩定 API。
- **簡報**：依序展示「痛點 → 使用者操作 → agent 的可見決策 → 結果 → 真實世界可行性」，最後才談技術架構。

## 五、資料使用提醒

本文件的台灣站資訊來自公開轉載之主辦公告；新加坡站的題目、評分與得獎案例是有根據的前例，但台灣站可能在現場提供不同的題目、工具、提交格式或評審標準。決賽前後若收到官方 email、現場簡報或官方網站更新，應以最新的主辦資訊覆蓋本文件。
