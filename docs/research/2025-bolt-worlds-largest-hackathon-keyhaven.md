# 2025 World’s Largest Hackathon presented by Bolt：KeyHaven（線上）

研究日期：2026-09-08
資訊分類：賽事、名次與作品頁面標記為「已確認資訊」；作者的技術過程、AI 使用與困難為作者本人第一手公開紀錄；其餘均明列為「研究推論」或「待確認問題」。本案例僅是參考性先例，不是 Sea × OpenAI 台灣站規則、產品需求或團隊決定。

## 研究問題

是否能找到一個 2024–2026 的 AI-assisted／vibe-coding 黑客松單一得獎案例，且同時有：主辦方直接確認的成果、可檢查作品，與作者本人對 AI 輔助開發過程、取捨或限制的公開復盤？

本筆記選擇 **KeyHaven**：它在 Bolt 官方 winners 頁列為「3rd place」，有作者公開的 Devpost 提交頁與 live demo，且作者 Tommy Thomas 在個人 DEV Community 文章說明 Bolt 如何協助架構、code generation、debug 與 backend 優化，也交代仍由人處理的安全與整合問題。

## 基本資料

| 欄位 | 可直接查驗的內容 |
| --- | --- |
| 賽事 | World’s Largest Hackathon presented by Bolt；Devpost 賽事頁列為線上、公開活動，並標示由 Devpost 管理。[來源](https://worldslargesthackathon.devpost.com/) |
| 時間 | Devpost 賽事頁列為 2025-05-30 至 2025-06-30。[來源](https://worldslargesthackathon.devpost.com/) |
| AI-assisted／vibe-coding 關聯 | 賽事要求新 app 主要以 Bolt.new 建立；提交時須確認使用 Bolt.new 並展示 `Built with Bolt.new` badge。Bolt 是以自然語言協助開發與部署的工具；本筆記不把其工具特性誤寫成其他賽事的規定。[來源](https://worldslargesthackathon.devpost.com/) |
| 研究對象 | KeyHaven 是作者描述的 API key（應視為敏感憑證）儲存、rotation、monitoring 平台；這是作品作者的描述，未經本研究獨立測試或安全稽核。[作品頁](https://devpost.com/software/keyhaven) |

## 狀態

**案例觀察**。此為單一外部案例，尚未構成跨案例候選、團隊已採納做法或 Sea × OpenAI 台灣站的任何規則。

## 來源

### 官方來源

- [Bolt 官方 Winners 頁](https://bolt.new/winners)：主辦方直接列出 KeyHaven 為 **3rd place**。
- [Devpost 官方賽事頁](https://worldslargesthackathon.devpost.com/)：賽事名稱、線上日期、Bolt 使用要求、submission 要件與由 Devpost 管理的資訊。

### 可檢查作品證據

- [KeyHaven 的 Devpost 作品提交頁](https://devpost.com/software/keyhaven)：可檢視作品敘述、demo 影片嵌入、畫面、技術堆疊、live app 連結及 `Winner 3rd Place` 標示。作品內容屬作者提交，不是主辦方對功能或安全的獨立認證。
- [KeyHaven live demo](https://keyhaven.netlify.app/)：作者從提交頁連出的公開部署網址。本研究未登入、註冊或實際操作，且其可用性可能隨時間變動。

### 作者第一手紀錄／補充來源

- [Tommy Thomas：Building KeyHaven: My Journey with Bolt](https://dev.to/0xtommythomas/building-keyhaven-my-journey-with-bolt-at-the-worlds-largest-hackathon-3k2j)：作者署名並在 2025-07-09 發布；描述 AI 使用、實作障礙與自己的判斷。
- [KeyHaven 的 Devpost 提交頁](https://devpost.com/software/keyhaven)：同一位作者以第一人稱補充使用 Bolt、Claude、ChatGPT，以及 Resend、Stripe 與跨瀏覽器動畫的困難。

## 已確認資訊

- Bolt 的官方 winners 頁將 **KeyHaven** 明確列為本場 **3rd place**；作品頁也在所屬賽事下顯示 `Winner 3rd Place`。[官方 winners](https://bolt.new/winners) [作品頁](https://devpost.com/software/keyhaven)
- Devpost 的賽事頁明定作品須為新的 application、主要以 Bolt.new 建立，並需提供約三分鐘的公開 demo、功能可用的公開 URL，以及確認 Bolt 使用的 badge。這些是該活動的直接規則，不是 Sea × OpenAI 台灣站規則。[賽事頁](https://worldslargesthackathon.devpost.com/)
- 作者的 Devpost 作品頁把 Tommy Thomas 列為 creator，並提供作品畫面、嵌入影片與 live demo URL；因此能查驗提交材料的存在，但不代表本研究已重現其行為或檢驗其安全性。[作品頁](https://devpost.com/software/keyhaven)
- 作者在 DEV Community 說，Bolt.new 讓他數分鐘內建立初始架構，並以 AI 功能產生程式片段、加快 debugging、調整 backend logic；他表示可因此集中處理 secure key management 的核心問題。[作者文章](https://dev.to/0xtommythomas/building-keyhaven-my-journey-with-bolt-at-the-worlds-largest-hackathon-3k2j)
- 同一作者也把「不中斷服務的 automated key rotation」列為主要難題，稱以 AI-assisted code generation 與 prompt-based debugging 來快速 prototype／test；提交頁另列出 Resend alerts、Stripe billing 與跨瀏覽器 animation performance 的實作障礙。[作者文章](https://dev.to/0xtommythomas/building-keyhaven-my-journey-with-bolt-at-the-worlds-largest-hackathon-3k2j) [作品頁](https://devpost.com/software/keyhaven)

## 參考性先例

### 關鍵決策、限制、取捨與結果

**來源直接陳述**：作者將 Bolt 作為 application logic／infrastructure 的主要工具，同時使用 Claude、ChatGPT 協助 ideation、程式改善與特定 implementation challenge。作者把 AI 描述為能加速架構建立、複雜邏輯、測試與 debug 的 coding partner；但他實際提到的 key rotation、email timing、billing flow、跨瀏覽器 rendering／performance 仍須測試與 troubleshooting。[作者文章](https://dev.to/0xtommythomas/building-keyhaven-my-journey-with-bolt-at-the-worlds-largest-hackathon-3k2j) [作品頁](https://devpost.com/software/keyhaven)

**研究推論**：這個案例顯示 AI coding tool 可以將樣板與局部除錯的時間轉移給產品／風險判斷，但沒有移除整合驗證的工作。這是單一作者的回顧，不證明特定工具、prompt 或開發速度必然能得獎。

### 候選實務一：以可驗證的核心風險，約束 AI 快速原型的功能範圍

- **來源證據：** 作者說 Bolt 讓初始架構與程式產生更快，卻把不影響服務的 key rotation 視為主要挑戰，並針對它 prototype／test；其他整合也需要反覆 troubleshooting。[作者文章](https://dev.to/0xtommythomas/building-keyhaven-my-journey-with-bolt-at-the-worlds-largest-hackathon-3k2j) [作品頁](https://devpost.com/software/keyhaven)
- **研究推論：** 當 AI 助理縮短建立介面的時間時，團隊日後可優先定義一條核心流程與一個最主要失敗風險，先用可重播的方式檢查，再擴張功能；這是可討論的候選，不是本隊既定流程。
- **何時值得討論：** 已取得本場主辦方規則，且團隊選擇了需連接第三方服務或有明顯失敗後果的 prototype 時。
- **未來規劃問題：** 核心 demo 成功的可觀測條件是什麼？哪一個整合失敗時必須誠實呈現限制，而不是模擬成功？
- **限制／風險：** KeyHaven 的 API key 場景特別敏感；其作者主張與 demo 不是 security audit。快速 prototype 不應被誤解為可安全處理真實憑證。
- **待核實項目：** 作者未公開 prompt、測試案例、成功率、未預期錯誤率或資安評估，無法量化這種做法的效果。

### 候選實務二：把 AI 當作加速工具，而非整合與產品責任的替代者

- **來源證據：** 作者明示同時以 Bolt、Claude、ChatGPT 支援不同階段的思考與實作；也明示 Resend、Stripe、動畫的相容性／效能問題需以 persistence、testing、community resources 處理。[作品頁](https://devpost.com/software/keyhaven)
- **研究推論：** AI-assisted coding 可能幫助小型團隊更快把想法變成可展示流程，但人仍需確認第三方服務行為、資料／權限界線、品質與 demo 真實性。
- **何時值得討論：** 團隊決定使用一個以上 AI coding assistant 或外部 API，且已有可被人實測的部署環境時。
- **未來規劃問題：** 每個外部服務的 owner、失敗提示與 demo 替代流程由誰確認？是否將 AI 的產出與人為驗收分開記錄？
- **限制／風險：** 此案例只有一位作者的自述，沒有對照組，也無法證明多工具並用優於單一工具；不同工具版本、信用額度和賽事條款可能改變結果。
- **待核實項目：** 此研究沒有檢查 KeyHaven 與任何外部服務的帳號設定、費用、資料傳輸、授權或 production readiness。

## 不可套用台灣站

- 這場賽事的日期、公開線上形式、主要使用 Bolt.new 的要求、demo 長度、公開 URL／badge 規定、獎項與資格限制，全是 **World’s Largest Hackathon presented by Bolt** 的資訊；不得視為 Sea × OpenAI 台灣站已確認規則。[賽事頁](https://worldslargesthackathon.devpost.com/)
- KeyHaven 的 API key management 題目、Bolt／Claude／ChatGPT 的搭配和作者的個人開發經驗，都不是本隊產品需求或授權使用的工具清單。
- 即使 Bolt 官方列 KeyHaven 為第三名，這也不證明未來其他賽事偏好同類題目、使用 AI coding tool 的程度，或相同 demo 作法。

## 待確認問題

- 本研究沒有獲得 KeyHaven 的公開 source repository；可查驗的是其 Devpost page、影片嵌入、畫面與 live demo。這符合「project page／demo」作品證據，但不能據此審查程式碼、依賴、security controls 或 license。
- live demo 可能下線、改版、要求登入或與提交期不同；本研究未執行其功能，也不對可用性、安全性或任何 API key handling 作保證。
- 作者文章是第一手復盤，但沒有公開完整開發時數、提示詞、原始 AI 對話、完整測試紀錄、demo 失敗紀錄或第三方評審 feedback；不能補造。
- Sea × OpenAI 台灣站是否允許或要求 AI coding assistant、外部 SaaS、公開 demo／repo、特定模型或資料處理方式，仍須等待台灣站主辦方的直接說明。

## 已驗證案例門檻判定

**符合。**

1. **主辦方直接成果證據**：Bolt 官方 winners 頁直接列 KeyHaven 為第三名；Devpost 賽事頁確認這是其 World’s Largest Hackathon presented by Bolt。[Bolt winners](https://bolt.new/winners) [賽事頁](https://worldslargesthackathon.devpost.com/)
2. **可檢查作品證據**：公開 Devpost project page 含影片／畫面及 live demo URL；不只是一篇得獎文字公告。[作品頁](https://devpost.com/software/keyhaven)
3. **作者第一手紀錄**：Tommy Thomas 署名的 DEV Community 文章與 Devpost 自述，明確描述 AI 工具如何被使用、有哪些取捨與哪些整合問題未被 AI 自動消除。[作者文章](https://dev.to/0xtommythomas/building-keyhaven-my-journey-with-bolt-at-the-worlds-largest-hackathon-3k2j) [作品頁](https://devpost.com/software/keyhaven)

這只表示本案例滿足研究記錄的三類可回查證據，不表示它已達跨案例門檻，亦不構成團隊採納。
