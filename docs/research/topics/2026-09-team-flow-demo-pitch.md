# 有限時間內的分工、收斂與 Demo Pitch

## 研究契約

- **批次 ID：** `2026-09-team-flow-demo-pitch`
- **日期：** 2026-09-09
- **執行進度：** 完成
- **核心研究問題：** 已驗證的黑客松團隊，如何在有限時間內安排工作分配、收斂工作範圍，並以 Demo 與 Pitch（上台簡報）完成展示？
- **適用範圍：** 僅整理其他賽事的參考性先例；不確認 Sea × OpenAI 台灣站規則，不替本隊採納角色、時程、流程或產品決策。

## 納入與排除

納入一個團隊／作品，須同時具有：

1. 主辦方官方得獎／入選公告，或官方提交平台的參賽證據；
2. 可檢查的作品證據，如 repository、Demo、作品頁、簡報或影片；
3. 團隊第一手公開紀錄，明示至少一項分工、工作流程、範圍取捨、Demo 或 Pitch 經驗。

排除只有新聞、轉貼、作品頁，或無法追溯團隊經驗的候選。最多篩選八案；目標三個已驗證案例，且優先跨至少兩場競賽。

## 共同比較問題

1. 團隊如何分配責任，哪些責任可由來源直接證實？
2. 如何將想法或工作範圍收斂為可展示的最小成果？
3. 哪些時間節點、返工或未完成風險改變了工作順序？
4. Demo 與 Pitch 如何安排故事、證據、操作與時間？
5. 發生失敗、放棄工具或 Demo 不穩時，團隊如何應對？
6. 哪些結論受賽制、隊伍規模或來源偏差限制？

## 來源要求、輸出與停止條件

- 每案獨立研究、獨立案例檔；不把多案混入同一次研究。
- 官方來源確認成果／參賽；作品來源確認可檢查內容；團隊第一手來源確認其經驗敘述。
- 預定輸出：三份 `docs/research/YYYY-event-project[-team].md` 案例檔、本摘要與研究總覽導覽更新。
- 篩選八案後若僅得兩案：標為「完成但證據受限」；一案或零案：標為「研究不完整」，不提出跨案例候選。

## 候選池與執行記錄

已篩選八案，納入三個已驗證案例，涵蓋三場競賽：

| 結果 | 案例 | 理由 |
| --- | --- | --- |
| 納入 | [Finny AI](../2026-nextgenhack-finny-ai.md) | 官方勝隊、可檢查 Demo 與團隊復盤齊備。 |
| 納入 | [Clean Runnings](../2022-hack-to-the-future-clean-runnings.md) | 官方獲獎／團隊訪談與公開 repository 齊備。 |
| 納入 | [Project Gandalf](../2020-nasa-space-apps-project-gandalf.md) | 官方提交頁、公開 repository 與團隊第一手流程齊備；未主張得獎。 |
| 排除 | MenuSnap、Pitch Perfect | 有官方與作品，但缺團隊第一手分工／流程紀錄。 |
| 排除 | Team sn@tch | 有官方訪談與第一手敘述，但缺可檢查作品。 |
| 反例／排除 | Breakin’ Into the Louvre、Tiffany 團隊 | 分別缺可檢查作品／官方成果；保留為來源不足的反例線索，不計入案例。 |

反例搜尋亦由各納入案例保留：Clean Runnings 的 Map API 首日失敗、Project Gandalf 的資料格式與缺漏問題；Finny AI 未提供可查的返工細節。未找到某類反例不代表其不存在。

## 比較

| 比較面向 | Finny AI | Clean Runnings | Project Gandalf | 可支持的限縮觀察 |
| --- | --- | --- | --- | --- |
| 分工 | development 與 product/pitch 並行，反覆合流。 | 先定 MVP，再由五人分配工作。 | Data Collection、Coding、Presentation 三組，依交付依賴串接。 | 三案皆有明示分工，但角色名稱、規模與時程不同。 |
| 收斂／風險 | 60 分鐘內選定單一問題。 | Map API 失敗後取捨功能，最後修復。 | 資料缺漏／過時，改找其他來源。 | 三案都受核心依賴限制；Gandalf 僅支持資料→模型→展示的依賴與末段整合，不證明此法必然較快或得獎。 |
| Demo／Pitch | 已部署 Demo 與 QR code 支撐 Pitch。 | 以實跑情境展示路線核心。 | 末段全隊 review、做 slideshow；Presentation 組參加 pitch boot-camp。 | 展示工作須與核心流程整合；各賽事的上台規格仍不同且未可互套。 |

## 跨案例候選

### 先以一條可展示的核心價值鏈安排工作，並為展示保留整合工作

- **支持案例：** [Finny AI](../2026-nextgenhack-finny-ai.md)、[Clean Runnings](../2022-hack-to-the-future-clean-runnings.md)、[Project Gandalf](../2020-nasa-space-apps-project-gandalf.md)。
- **研究推論：** 三案都將分工連回一條能說明問題、核心處理與展示的鏈；Finny AI 的 product/pitch 並行、Clean Runnings 的實跑 demo、Project Gandalf 的末段整合，支持展示需要與核心成果相連，而非只獨立描述想法。
- **適用條件與限制：** 僅為跨三場賽事的候選，不代表固定職稱、固定人數、固定時程或勝因。Finny AI 的返工資料不足；Project Gandalf 的 slides 無法檢查內容。
- **反例／不支持：** Clean Runnings 的 API 故障與 Project Gandalf 的資料問題顯示，將展示納入分工或末段整合並不能避免核心依賴失敗；來源不足的排除案例不能反證或支持此候選。
- **狀態：** 跨案例候選，未獲團隊採納。

### 優先驗證最會使核心展示中斷的外部依賴，並保留可誠實說明的範圍邊界

- **支持案例：** [Clean Runnings](../2022-hack-to-the-future-clean-runnings.md)、[Project Gandalf](../2020-nasa-space-apps-project-gandalf.md)。
- **研究推論：** 兩案都因地圖 API 或資料品質改變工作；可討論在早期辨識高風險依賴與其降級邊界，避免把未證實能力放入 Pitch 主張。
- **適用條件與限制：** 僅有兩案直接支持，且並未證實預先的 fallback 已存在或保證成功；不可把加班、臨場修復或犧牲功能描述為可複製策略。
- **反例／不支持：** Finny AI 未提供相應外部依賴失敗或 fallback 證據，故不納入支持計數。
- **狀態：** 跨案例候選，未獲團隊採納。

## 台灣站不可套用

其他賽事的題目、工具、時程、評分、資格、獎項與資料規則，皆不是 Sea × OpenAI 台灣站已確認資訊。任何新加坡站資訊亦只可作參考性先例。

## 結論狀態

尚無結論。後續僅可能形成「案例觀察」或符合門檻的「跨案例候選」；未獲團隊明確決定前，均非團隊已採納做法。
