# 2025 ElevenLabs x a16z Worldwide Hackathon（線上組）

研究日期：2026-09-08
資訊標記：本筆記將來源可直接支持的內容標為「已確認資訊」；我自己的解讀另標為「研究推論」，尚待主辦方或團隊決定的內容標為「未決問題」。

## 研究範圍

本筆記研究 ElevenLabs 與 a16z 等夥伴合作的 **ElevenLabs Worldwide Hackathon** 之「Online hackathon winners」組別。它是 AI／agent 類的公開先例，特別適合用來觀察短時程、多模態（語音、視覺、agent）prototype 的公開復盤。

候選賽事也包含 Google 的 Gemini API Developer Competition 與 Google Cloud ADK Hackathon；本場入選，因為主辦方公告同時給出 40 小時、賽事規模與線上組前三名，而且獲獎作品頁保留 demo、技術材料與作者寫下的挑戰／取捨。本筆記只選線上組前三名，不把它誤稱為整個 Worldwide Hackathon 的全部獎項。

這不是 Sea×OpenAI 台灣站的已確認資訊，不是其規則、評分標準或產品需求。

## 狀態

案例觀察；尚未形成團隊決策或 Sea×OpenAI 台灣站規則。

## 直接來源

- [ElevenLabs：Worldwide Hackathon 得獎公告（2025-02-28）](https://elevenlabs.io/blog/announcing-the-winners-of-the-elevenlabs-worldwide-hackathon)：主辦方公告；包括賽事規模、40 小時與線上組名次。
- [Hugo Tour Guide：Devpost 作品提交頁](https://devpost.com/software/hugo-tour-guide)：第一名作品作者／團隊的功能、技術、demo、repository、workflow 與復盤。
- [Pep：Devpost 作品提交頁](https://devpost.com/software/pep-your-compassionate-physical-therapy-agent)：第二名作品作者／團隊的技術與挑戰復盤。

## 補充來源

- [官方 Devpost project gallery](https://elevenlabs-worldwide-hackathon.devpost.com/project-gallery)：賽事作品庫入口；可與個別作品頁交叉核對作品所屬賽事，但主辦方公告才是名次的主要證據。
- [Agent SFX：Devpost 作品提交頁](https://devpost.com/software/agent-sfx)：線上組第三名的可檢視作品證據與技術材料。

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

**可檢視作品證據**：作者在 [Devpost 作品頁](https://devpost.com/software/hugo-tour-guide) 提供嵌入式 demo、可點擊的 live app、前後端 repository 與 agent workflow；並描述 React 前端、Python 後端、ElevenLabs、OpenAI 與 Google Maps 的分工。這些是作品作者的公開材料，並非獨立審計。

### Pep – your compassionate Physical Therapy Agent（線上組第二名）

**官方得獎／成果證據**：ElevenLabs 公告將 Pep 列為線上組第二名，描述為以語音與視覺提供即時物理治療 coaching 的多模態 agent。[公告](https://elevenlabs.io/blog/announcing-the-winners-of-the-elevenlabs-worldwide-hackathon)

**可檢視作品證據**：[Devpost 作品頁](https://devpost.com/software/pep-your-compassionate-physical-therapy-agent)提供嵌入式 demo 與 GitHub 連結；作者說明它是 Swift iOS app，使用 ElevenLabs Conversation AI Swift SDK 與 Apple Vision Framework 做即時互動／關節辨識。

### Agent SFX（線上組第三名）

**官方得獎／成果證據**：ElevenLabs 公告將 Agent SFX 列為線上組第三名，描述它以 vision model 分析遊戲 node structure 與 screenshot，再產生 voiceover 和 sound effects。[公告](https://elevenlabs.io/blog/announcing-the-winners-of-the-elevenlabs-worldwide-hackathon)

**可檢視作品證據**：[Devpost 作品頁](https://devpost.com/software/agent-sfx)提供作品技術說明及外部 GitHub 連結。此案例列作補充，不據此推斷團隊成員資訊，因公告與提交頁的姓名呈現可能不同。

## 參賽經驗

以下是作品作者在提交頁的第一手公開經驗，不是主辦方背書的通用建議，也不能直接套用到其他賽事。

### 經驗一：Hugo 對 context／功能範圍的取捨

- **來源證據**：[Hugo Tour Guide 的 Challenges、Accomplishments、What We Learned](https://devpost.com/software/hugo-tour-guide)。這是第一名作者／團隊在提交頁留下的公開復盤。
- **公開經驗**：團隊說，context 資料很多時，LLM 難以遵從指令；因此刪減 guidelines、縮小 scope 來讓行為穩定。他們也表示在兩天內由大範圍想法收斂至 core value，交付較精簡的體驗；語音介面則測試多個 TTS API，認為回應速度與語言正確性重要。
- **可供未來討論的問題**：在固定時間內，哪一條從輸入到輸出的核心體驗最值得先完成？可接受哪些 context、功能或語言先不做？語音 demo 要如何測量「反應夠快、語言正確」？
- **限制／風險**：這是單一得獎團隊的敘述，沒有量化測試或反例，也可能受賽後敘事影響；「減 scope」不是在所有題目都正確的答案。
- **待核實項目**：其所稱的 prompt／語音改善沒有公開 benchmark、延遲數據或使用者研究；若未來需要做相似功能，應自建測試情境與可觀測指標。

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
