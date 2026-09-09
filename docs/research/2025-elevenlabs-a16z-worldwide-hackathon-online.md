# 2025 ElevenLabs x a16z Worldwide Hackathon（線上組）

研究日期：2026-09-09
資訊標記：本筆記將來源可直接支持的內容標為「已確認資訊」；我自己的解讀另標為「研究推論」，尚待主辦方或團隊決定的內容標為「未決問題」。

## 研究範圍

本筆記研究 ElevenLabs 與 a16z 等夥伴合作的 **ElevenLabs Worldwide Hackathon** 之「Online hackathon winners」組別。它是 AI／agent 類的公開先例，特別適合用來觀察短時程、多模態（語音、視覺、agent）prototype 的公開復盤。

候選賽事也包含 Google 的 Gemini API Developer Competition 與 Google Cloud ADK Hackathon；本場入選，因為主辦方公告同時給出 40 小時、賽事規模與線上組前三名，而且獲獎作品頁保留 demo、技術材料與作者寫下的挑戰／取捨。本筆記只選線上組前三名，不把它誤稱為整個 Worldwide Hackathon 的全部獎項。

這不是 Sea×OpenAI 台灣站的已確認資訊，不是其規則、評分標準或產品需求。

## 狀態

案例觀察；尚未形成團隊決策或 Sea×OpenAI 台灣站規則。

## 研究問題

Hugo Tour Guide 如何在此一得獎 prototype 中收斂功能範圍，使 MVP（Minimum Viable Product，最小可行產品）／demo 能清楚呈現價值？本檔只把團隊公開復盤當作單一 **參考性先例**，不把它升格為本隊做法或台灣站規則。

## 基本資料

- 賽事／組別：2025 ElevenLabs Worldwide Hackathon 的 global virtual chapter（本文沿用官方章節名「Online hackathon winners」）。
- 案例：Hugo Tour Guide；官方列為 Online 1st Prize。團隊為 Yilun Sun、Qiang Fang、David Chen、Aiden Zhao，四人、California, USA。[官方公告](https://elevenlabs.io/blog/announcing-the-winners-of-the-elevenlabs-worldwide-hackathon)
- 覆核結果：符合「已驗證案例」的三項所需證據：官方得獎公告、可檢查作品材料、團隊第一手公開復盤。最後查閱：2026-09-09。

## 來源

### 官方來源

- [Announcing the winners of the ElevenLabs Worldwide Hackathon](https://elevenlabs.io/blog/announcing-the-winners-of-the-elevenlabs-worldwide-hackathon) — ElevenLabs，作者 Louis Jordan、Thor Schaeff，主辦方公告，2025-02-28；查閱 2026-09-09。可直接確認賽事、Online 1st Prize、Hugo 功能摘要與四名成員。

### 可檢查作品證據

- [Hugo Tour Guide](https://devpost.com/software/hugo-tour-guide) — Hugo 團隊的 Devpost 提交頁（頁面列 Qiang Fang、Yilun Sun、Aiden Zhao、David Chen 為 creators；Qiang Fang 有提交更新），官方提交平台頁，查閱 2026-09-09。頁面可檢視功能說明、嵌入式影片與 live-app、前後端 repo 連結、agent workflow 連結、技術說明及團隊復盤。
- [前端 repository：drbearcub/hugo](https://github.com/drBearcub/hugo) — 公開 GitHub repository，查閱 2026-09-09。README 自稱 Hugo Tour Guide 前端、連到官方得獎公告、影片與 backend repo；可檢查但不是主辦方名次證據。
- [後端 repository：FWQ1234/voice_view_backend](https://github.com/FWQ1234/voice_view_backend) — 公開 GitHub repository，查閱 2026-09-09。由作品提交頁連結；可檢查但未在 README 自行說明 Hugo 關聯，故只作輔助作品材料。
- [Demo video](https://www.youtube.com/watch?v=ysKjLtJra-g) — 由 Devpost 嵌入／前端 README 連結的公開影片，查閱 2026-09-09；可播放性會隨平台變動，本次僅確認其連結存在。

### 補充來源／待核實來源

- [官方 Devpost project gallery](https://elevenlabs-worldwide-hackathon.devpost.com/project-gallery) — 賽事作品庫入口，查閱 2026-09-09；可交叉確認作品所屬賽事，但官方公告才是名次的主要證據。
- [Pep：Devpost 作品提交頁](https://devpost.com/software/pep-your-compassionate-physical-therapy-agent) 與 [Agent SFX：Devpost 作品提交頁](https://devpost.com/software/agent-sfx) — 同場其他名次的補充案例，不用來證明 Hugo 的取捨。

## 來源事實

### 已確認資訊

- ElevenLabs 公告稱，這是與 a16z 和其他夥伴共同舉辦的首屆 Worldwide Hackathon；開發者／builder 在六場實體活動、三場社群活動與全球 Discord 虛擬活動中，以 **40 小時**打造逾 300 個 AI agent 專案。[來源](https://elevenlabs.io/blog/announcing-the-winners-of-the-elevenlabs-worldwide-hackathon)
- 同一份主辦方公告把線上組名次列為：第一名 **Hugo Tour Guide**、第二名 **Pep – your compassionate Physical Therapy Agent**、第三名 **Agent SFX**。[來源](https://elevenlabs.io/blog/announcing-the-winners-of-the-elevenlabs-worldwide-hackathon)
- 主辦方對 Hugo 的摘要是個人化、location-aware 的 conversational travel agent；對 Pep 的摘要是結合 voice 和 vision、提供即時物理治療 coaching 的 agent；對 Agent SFX 的摘要是為遊戲開發者生成 voiceover 與 sound effects 的工具集合。[來源](https://elevenlabs.io/blog/announcing-the-winners-of-the-elevenlabs-worldwide-hackathon)

### 查無直接來源

- 在上述可直接取得的公告中，未找到完整 judging rubric、每隊分數、台灣站適用規則或每個 prototype 的獨立測試報告。因此不可把三件作品的做法當作通用得獎公式或安全／醫療效果證明。

## 作品案例

### Hugo Tour Guide（線上組第一名）

**官方得獎／成果證據**：ElevenLabs 將 Hugo 列為線上組第一名，並指出它規劃路線、回答在地文化／歷史問題並提供地圖。[公告](https://elevenlabs.io/blog/announcing-the-winners-of-the-elevenlabs-worldwide-hackathon)

**可檢視作品證據**：[Devpost 作品頁](https://devpost.com/software/hugo-tour-guide)保留嵌入式影片、live-app 與前後端 repo／workflow 連結；[前端 repo](https://github.com/drBearcub/hugo)的 README 亦標示為 Hugo Tour Guide 前端。作品頁描述 React 前端、Python 後端、ElevenLabs、OpenAI 與 Google Maps 的分工。這些是公開作品材料，並非獨立審計；本次未執行 app 或程式。

### Pep – your compassionate Physical Therapy Agent（線上組第二名）

**官方得獎／成果證據**：ElevenLabs 公告將 Pep 列為線上組第二名，描述為以語音與視覺提供即時物理治療 coaching 的多模態 agent。[公告](https://elevenlabs.io/blog/announcing-the-winners-of-the-elevenlabs-worldwide-hackathon)

**可檢視作品證據**：[Devpost 作品頁](https://devpost.com/software/pep-your-compassionate-physical-therapy-agent)提供嵌入式 demo 與 GitHub 連結；作者說明它是 Swift iOS app，使用 ElevenLabs Conversation AI Swift SDK 與 Apple Vision Framework 做即時互動／關節辨識。

### Agent SFX（線上組第三名）

**官方得獎／成果證據**：ElevenLabs 公告將 Agent SFX 列為線上組第三名，描述它以 vision model 分析遊戲 node structure 與 screenshot，再產生 voiceover 和 sound effects。[公告](https://elevenlabs.io/blog/announcing-the-winners-of-the-elevenlabs-worldwide-hackathon)

**可檢視作品證據**：[Devpost 作品頁](https://devpost.com/software/agent-sfx)提供作品技術說明及外部 GitHub 連結。此案例列作補充，不據此推斷團隊成員資訊，因公告與提交頁的姓名呈現可能不同。

## 參賽經驗

以下是作品作者在提交頁的第一手公開經驗，不是主辦方背書的通用建議，也不能直接套用到其他賽事。

### 經驗一：Hugo 對 context／功能範圍的取捨

- **來源證據：**[Hugo Tour Guide 提交頁的 Challenges 與 Accomplishments](https://devpost.com/software/hugo-tour-guide)，作者／團隊第一手公開復盤（頁面列 creator 與 Qiang Fang 的提交更新；查閱 2026-09-09）。團隊直接寫到：大量 context 令 LLM 難以遵從指令，遂刪減 guidelines、縮小 scope；又由大範圍想法快速收斂 core value，交付 streamlined experience。這可驗證「團隊曾如此陳述」，不驗證效能或因果。
- **研究推論：**此案例可形成的單案觀察是：當一個核心流程受 context／指令穩定性拖累時，團隊選擇移除部分指南與範圍，讓 demo 聚焦於其辨識出的核心價值。這不是「少做必然得獎」的規則。
- **何時值得討論：**短時程 prototype 已能指出一條端到端核心流程、但擴充情境使回應不穩或難以講清楚時。
- **未來規劃問題：**哪一條從使用者輸入到可見結果的流程最能說明價值？哪些 context、情境或語言先暫緩，且如何明示 demo 的邊界？如何另行測試語音回應速度與語言正確性？
- **限制／風險：**單一得獎隊伍的自述，無量化 benchmark、延遲數據、使用者研究或反例；作品頁同時列出多項功能，無法從公開材料判定 demo 實際啟用範圍及裁判採用何準則。
- **待核實項目：**尚無團隊逐項功能刪留清單、版本歷史、demo 觀看／評分紀錄，或裁判對「範圍收斂」的直接評語；故不可推導此取捨造成得獎。

### 經驗二：Pep 對 SDK 穩定性與多 agent session 的復盤

- **來源證據**：[Pep 的 Challenges 與 What we learned](https://devpost.com/software/pep-your-compassionate-physical-therapy-agent)。這是第二名作者／團隊在提交頁的直接說明。
- **公開經驗**：作者表示 Swift SDK 的 memory safety 與 Vision Framework／AVFoundation 整合在 iPhone 測試時造成 crash；把兩個 agent 串起來時，也必須處理 context switching 與 session persistence。作者另提到文件與 tool-use template 不足，增加了 debug 時間。
- **可供未來討論的問題**：若作品仰賴新 SDK、多模態或多 agent，最早該驗證的整合點是什麼？context 與 session 要由誰管理？遇到 crash 時要保留哪一條可演示的備援流程？
- **限制／風險**：這是作者對當時版本工具鏈的經驗，不代表 SDK 現況，也不代表所有 iOS／vision 組合都會失敗；它也不是臨床物理治療的有效性證據。
- **待核實項目**：若重做相近原型，需重新核實 SDK 版本、裝置相容性、資料處理、使用者安全與醫療相關聲明的界線。

## 研究推論

- **研究推論，不是需求**：此場的得獎案例共同有一個容易描述的核心任務（旅遊引導、復健陪練、遊戲聲音資產），並以多模態／agent 元件服務這個任務；未來可討論「能否用一個具體流程說明 AI 的價值」，而非先堆疊工具名。
- **研究推論，不是規則**：Hugo 與 Pep 的復盤都把整合穩定性列為實作阻力，因此在短時程 prototype 中，先驗證高風險整合、再擴大功能，可能降低 demo 失敗機率。
- **研究推論，不是安全保證**：涉及語音、健康或個人化建議時，demo 的可行性不等於能安全提供真實世界服務；資料、同意、錯誤處理與責任界線仍須個別評估。

## 限制與本隊差異

- 這是全球、含線上與實體章節的 40 小時活動；Sea×OpenAI 台灣站的時程、資格、題目、評分、可用 API、資料與獎項均未由本筆記假定為相同。
- 主辦方公告列出多個獎項層級。本筆記只分析明確標為「Online hackathon winners」的前三名，避免混淆 global top prize、partner prizes 與城市獎項。
- Devpost 提交頁是作者自行公開的展示與復盤；live demo、repo、文件可能失效或在賽後變更。本研究未執行程式、沒有做資安／品質／法規審查。
- Pep 是健康相關 prototype，但本筆記不把它當成醫療建議、醫療器材或效果證據。

## 未決問題

- Sea×OpenAI 台灣站是否有明確限制、主題、評分面向、團隊規模、時程與提交格式？需等待主辦方直接說明，不能以本場代替。
- 本隊若選多 agent 或多模態，哪個最小可展示流程最值得驗證？這仍是產品與實作討論，尚未決定。
- 哪些資料類型與健康／語音情境會出現在未來構想中？在確認前，不能把上述 prototype 的做法搬成需求。
