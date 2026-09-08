# 2024 Google Gemini API Developer Competition（線上）

研究日期：2026-09-08
資訊標記：本筆記把可由主辦方或作品作者直接支持的內容標為「已確認資訊」；其餘明確標為「研究推論」或「未決問題」。

## 研究範圍

本筆記研究 Google 主辦的 **Gemini API Developer Competition**（2024、全球線上徵件），只為未來討論提供可回查的先例，不是 Sea×OpenAI 台灣站的規則、評分標準或產品決策。

候選賽事曾包含 Google Cloud 的 2025 ADK Hackathon 與 ElevenLabs x a16z 的 2025 Worldwide Hackathon；本場入選，是因為同時能取得：主辦方得獎公告、官方作品頁、作者公開程式碼與作者本人復盤。它也提供一個「單人、螢幕互動型 AI assistant」的不同案例。

## 狀態

案例觀察；尚未形成團隊決策或 Sea×OpenAI 台灣站規則。

## 直接來源

- [Google Developers：得獎公告（2024-11-21）](https://developers.googleblog.com/en/announcing-the-winners-of-the-gemini-api-developer-competition/)：主辦方的得獎名單與作品摘要。
- [Google AI for Developers：Jayu 官方作品頁](https://ai.google.dev/competition/projects/jayu?hl=en)：主辦平台收錄的得獎作品說明；功能、架構與安全界線是參賽者提交內容，並非 Google 的獨立測試結果。
- [Jayu 作者公開 repository](https://github.com/JonOuyang/Gemini-Computer-Use)：作者公開的可檢視程式碼與 demo 連結。
- [Jonathan Ouyang 的賽後公開貼文](https://www.linkedin.com/posts/jon-ouyang_google-googlegemini-activity-7265418646028447745-uqkA)：作品作者的第一手復盤。

## 補充來源

- [Google AI for Developers：全體得獎作品頁](https://ai.google.dev/competition?hl=en)：賽期為 2024-05-15 至 2024-08-12，並列出各獎項。
- [Google Blog：如何使用 Gemini API 的得獎摘要](https://blog.google/innovation-and-ai/technology/developers-tools/gemini-api-developer-competition-winners/)：Google 對參賽作品類型的補充介紹。

## 來源事實

### 已確認資訊

- Google 的全體得獎頁說明，這場競賽在 2024-05-15 至 2024-08-12 挑戰開發者以 Gemini API 製作 app；這是本筆記所稱「2024 賽事」的依據。[來源](https://ai.google.dev/competition?hl=en)
- 主辦方在 2024-11-21 的公告把 **Jayu** 列為「Best Overall App」，並連結作品 demo。[來源](https://developers.googleblog.com/en/announcing-the-winners-of-the-gemini-api-developer-competition/)
- 官方作品頁收錄的參賽者作品說明，將 Jayu 描述為能以螢幕畫面為 context、與畫面元素互動的個人 assistant；該說明稱其以 Flash 作 command center，必要時透過 function calling 使用其他 Gemini model。[來源](https://ai.google.dev/competition/projects/jayu?hl=en)
- 同一份參賽者作品說明也宣稱其安全界線為：只在使用者直接要求時看螢幕、不存留影像或錄音記憶，且不應存取未顯示的資料夾或 app；研究未對這些宣稱做獨立驗證。[來源](https://ai.google.dev/competition/projects/jayu?hl=en)

### 查無直接來源

- 本次查到的主辦公告與官方作品頁沒有公布完整評分 rubric、每位評審的評語，或與台灣站相同的題目／時程；因此不能把本案的技術選擇解讀成其他賽事的得分公式。

## 作品案例

### Jayu（Best Overall App）

**官方得獎／成果證據**：Google 的得獎公告直接列出 Jayu 為 Best Overall App，並描述它可以解讀視覺資訊、操作 application interface 和進行即時翻譯。[公告](https://developers.googleblog.com/en/announcing-the-winners-of-the-gemini-api-developer-competition/)

**可檢視作品證據**：

- [官方作品頁](https://ai.google.dev/competition/projects/jayu?hl=en)可檢視產品敘述、系統分工與安全界線。
- [作者公開 repository](https://github.com/JonOuyang/Gemini-Computer-Use)提供公開程式碼，README 連到 demo video，且作者明示 Jayu 是自己為本競賽製作的作品。

**已確認的作品取捨**：官方作品頁描述的不是「無限制電腦控制」，而是以正在顯示且被直接要求分析的畫面為邊界；這是一項作品作者／團隊公開說明的範圍選擇，不是安全性已獨立驗證的結論。[來源](https://ai.google.dev/competition/projects/jayu?hl=en)

## 參賽經驗

以下是參賽者自己的公開敘述；它們是個別經驗，不可泛化為所有參賽者都會如此，也不是 Sea×OpenAI 台灣站的規則。

### 經驗一：單人投入與 demo 迭代

- **來源證據**：[Jonathan Ouyang 的賽後貼文](https://www.linkedin.com/posts/jon-ouyang_google-googlegemini-activity-7265418646028447745-uqkA)；發文者自稱 Jayu 唯一開發者。
- **公開經驗**：他表示自己在入學前的暑假投入約兩個月開發，並請多位 reviewer 反覆觀看 demo 直到滿意；同一篇也描述壓力、睡眠受影響與多次情緒崩潰。這同時顯示他把可展示性投入得很深，也顯示代價。
- **可供未來討論的問題**：若團隊資源有限，demo 的可理解性、可重播性與核心功能完成度之間，如何分配時間？是否要預先設定工作時數與休息界線？
- **限制／風險**：這是得獎單人作者的自述，沒有對照組，不能推出「投入兩個月」或「大量 demo review」會帶來勝利；亦不應浪漫化過度工作。
- **待核實項目**：貼文提到的提交數、參與開發者數、觀看數與私人健康感受，不應作為競賽官方統計；若日後需要正式統計，應另找主辦方可驗證的資料。

### 經驗二：把功能野心置於可控制的資料邊界內

- **來源證據**：[Jayu 官方作品頁](https://ai.google.dev/competition/projects/jayu?hl=en)與[作者公開 repository](https://github.com/JonOuyang/Gemini-Computer-Use)。前者收錄作品提交內容，後者是作者公開的技術說明。
- **公開經驗**：作者將螢幕理解、語音與手勢放進同一個 assistant，同時宣告只處理 active window、依使用者指示才擷取螢幕、session 間不保留記憶等界線。[官方作品頁](https://ai.google.dev/competition/projects/jayu?hl=en)
- **可供未來討論的問題**：對會讀取畫面、語音或個人內容的 prototype，哪些「可以看／不可以看／何時看／是否保留」界線要先說清楚，才能讓 demo 容易理解且降低誤用？
- **限制／風險**：這些是作品自我宣告的設計與 repository 說明，研究沒有執行程式或進行資安審計；不能因此主張它符合任何實際部署的隱私或安全要求。
- **待核實項目**：若未來想採用相近互動，仍需核實目標 OS 權限、資料傳輸、第三方服務條款、使用者同意與實際測試結果。

## 研究推論

- **研究推論，不是需求**：這個案例讓人看到，AI-native 作品可以把 model capability 與一條具體可見的使用流程綁在一起（例如「看目前畫面後完成一個互動」），而不是只展示泛用聊天。
- **研究推論，不是規則**：當作品接觸敏感的畫面或語音時，把互動範圍寫得具體，可能有助於評審與使用者理解 demo 的界線；此推論不能取代真正的安全測試。
- **研究推論，不是工作方式要求**：作者的壓力自述提醒團隊要把健康、時間盒（timebox）與停止條件納入未來討論，而不是複製其工作強度。

## 限制與本隊差異

- 本場為全球線上 competition；尚無直接來源證明它和 Sea×OpenAI 台灣站在題目、資格、工具、評分、時程、獎項或資料權利上相同。
- Jayu 的場景涉及桌面畫面、語音與手勢；未來本隊是否採用任何一種互動仍是未決產品選擇。
- repository 與 demo 是作者公開資料，但可能已隨時間改版或失效；本筆記只記錄研究當日可見的證據，不保證可重現。
- 本筆記沒有執行 Jayu、驗證效能，或審查其安全／法規合規性。

## 未決問題

- 主辦方是否曾公開完整評分 rubric 與 finalist feedback？本次查無直接來源。
- 若未來比賽允許 computer-use 類能力，會如何要求權限說明、demo 資料與安全測試？需等待主辦方規則或另做查證。
- 本隊是否有明確使用者、可展示的一步流程與可接受的資料界線？這是產品討論，現階段不在本筆記中替團隊決定。
