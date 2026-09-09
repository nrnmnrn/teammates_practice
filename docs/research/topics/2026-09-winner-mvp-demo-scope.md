# 得獎隊伍的 MVP 與 Demo 範圍收斂

## 研究契約

- **批次 ID：** 2026-09-winner-mvp-demo-scope
- **日期：** 2026-09-09
- **執行進度：** 完成
- **核心研究問題：** 已得獎隊伍如何取捨功能範圍，使 MVP（最小可用作品）與 Demo（展示）能清楚呈現核心價值？
- **適用範圍：** 公開可驗證之黑客松得獎作品；僅研究參考性先例。
- **不可套用台灣站：** 他賽規則、時程、評分、資格、工具與獎項，均非 Sea × OpenAI 台灣站已確認資訊。

## 納入與排除

- **納入：** 每案具官方得獎證據、可檢查作品證據，及參賽者／團隊公開第一手紀錄；紀錄須談及功能取捨、核心體驗、Demo 或返工之一。
- **排除：** 僅有新聞或轉貼、無法檢查作品、無第一手紀錄，或只宣稱得獎而無官方佐證之案例。
- **目標：** 最多篩選八案，納入三個已驗證案例，至少跨兩場競賽。

## 共同比較問題

1. 團隊如何定義唯一優先展示的核心價值？
2. 哪些功能、資料範圍或互動被延後、刪除或限制？
3. 此取捨如何影響 MVP 與 Demo 的可理解性、穩定性或完成度？
4. 第一手紀錄是否提及返工、失敗展示、技術限制或代價？
5. 該經驗的證據邊界與不可泛化處何在？

## 來源要求與停止條件

- **每案來源組合：** 主辦方官方成果公告；作品頁、repository、demo、簡報或影片之一；作者／團隊第一手紀錄。
- **反例搜尋：** 搜尋公開之返工、失敗 Demo、放棄功能或未完成經驗；未找到不表示不存在。
- **停止：** 三案皆獲驗證，或篩滿八案後僅餘兩案，或來源不足而無法形成跨案例候選。

## 候選池與執行記錄

| 候選 | 結果 | 理由 |
| --- | --- | --- |
| Jayu，Google Gemini API Developer Competition 2024 | 納入 | 官方得獎、官方作品頁／repository／影片、作者復盤俱在。見[案例](../2024-google-gemini-api-developer-competition-online.md)。 |
| Hugo Tour Guide，ElevenLabs × a16z Worldwide Hackathon 2025 | 納入 | 官方第一名、作品頁／repository／demo、團隊復盤俱在。見[案例](../2025-elevenlabs-a16z-worldwide-hackathon-online.md)。 |
| KeyHaven，World’s Largest Hackathon presented by Bolt 2025 | 納入 | 官方第三名、作品頁／影片／live URL、作者復盤俱在。見[案例](../2025-bolt-worlds-largest-hackathon-keyhaven.md)。 |
| A Web Monetization Story，Betahack | 反例線索，不納入正式案例 | 團隊提交頁描述 scope creep 與重剪 demo；本批未逐案完成官方成果與作品／第一手來源覆核，故不計數。[線索](https://devpost.com/software/borzoi) |
| Smartopia，What The Hack 2018 | 反例線索，不納入正式案例 | 自述功能過廣及展示受限，且未得獎；可供日後另案覆核。[線索](https://devpost.com/software/smartopia) |
| Contact Catalyst，Databricks Asia Pacific LLM Cup 2023 | 候補 | 有官方得獎、提交與作者貼文線索；本批三案已達停止條件，未立正式案例檔。[官方線索](https://databricks-llm-cup-2023.devpost.com/updates/28385-and-the-winners-are) |
| Hermes Protocol，NEARCON IRL Hackathon 2022 | 排除 | 缺較完整的作者賽後範圍／Demo 復盤。 |
| Firewallet，ETHIndia 2022 | 排除 | 第一手材料偏作品說明，未足以回答本批取捨問題。 |

反例搜尋方向為返工、失敗 Demo、放棄功能與未完成經驗。找到兩個線索；未逐案覆核，故不能作跨案例證據，也不能因未找到更多反例而推論其不存在。

## 比較

| 案例 | 可回查的核心價值／邊界 | 可回查的範圍取捨 | 證據限制 |
| --- | --- | --- | --- |
| [Jayu](../2024-google-gemini-api-developer-competition-online.md) | 畫面理解與直接互動；只看使用者明示的 active window。 | 有資料與操作界線；無公開功能刪減史。 | 未知哪些修改令 Demo 更好，亦未知評審因何給獎。 |
| [Hugo](../2025-elevenlabs-a16z-worldwide-hackathon-online.md) | 由廣泛旅遊想法收斂至 core value。 | 團隊明說刪 guidelines、縮 scope，以處理 context 不穩。 | 無刪留清單、量化測試或評審回饋。 |
| [KeyHaven](../2025-bolt-worlds-largest-hackathon-keyhaven.md) | 管理 API key；以不中斷 rotation 為高風險核心流程。 | 未找到功能刪減或 Demo 腳本收斂紀錄。 | 未操作 live app；無 source repository、成功率或安全稽核。 |

## 跨案例候選

### 先以一條可示範的價值鏈與明示邊界組織展示

- **狀態：** 跨案例候選；未獲團隊採納。
- **支持案例：** Jayu 將展示限於使用者直接要求的目前畫面互動；Hugo 由大範圍想法收斂 core value；KeyHaven 將作品敘述圍繞 API key 管理與不中斷 rotation。詳見三份[案例](../2024-google-gemini-api-developer-competition-online.md)、[案例](../2025-elevenlabs-a16z-worldwide-hackathon-online.md)、[案例](../2025-bolt-worlds-largest-hackathon-keyhaven.md)。
- **研究推論：** 先令觀者看見一個輸入至結果的核心流程，再清楚說明不處理什麼，可能較利於短時程展示的理解與查驗。
- **適用條件：** 能辨認一條端到端核心流程，且額外情境會稀釋敘事或降低穩定性時。
- **限制：** 三案皆不能證明此法造成得獎；Jayu 與 KeyHaven 沒有公開功能刪減史，故不可把「功能愈少愈好」當結論。

### 核心流程不穩時，把收斂視為可檢驗的取捨

- **狀態：** 跨案例候選；未獲團隊採納。
- **支持案例：** Hugo 明說 context 過多使 LLM 難以遵從指令，遂刪 guidelines、縮 scope；Jayu 的公開資料與操作邊界則提供另一種限制複雜度的做法。見[Hugo 案例](../2025-elevenlabs-a16z-worldwide-hackathon-online.md)與[Jayu 案例](../2024-google-gemini-api-developer-competition-online.md)。
- **研究推論：** 若擴充情境已使核心流程不穩，可先縮小 context、功能或可見互動範圍，並以可重播 Demo 檢查核心價值是否仍清楚。
- **反例與限制：** Smartopia 與 Betahack 只屬未覆核線索，不能證實或反駁此候選。亦無資料顯示各得獎隊的完整刪留順序。

## 台灣站不可套用與待確認問題

- 上述皆為其他賽事的參考性先例，不是 Sea × OpenAI 台灣站的時程、評分、繳交格式、工具或資格規則。
- 主辦方對台灣站換題問題尚未回覆；因此本批任何候選均不升級為產品或實作決策。
- 尚待團隊另行決定：何謂本隊可驗證的核心流程、如何測試 Demo、以及工作量與休息界線。

## 結論狀態

三個已驗證案例，跨三場競賽；本批**完成**。提出兩項跨案例候選；均未採納。
