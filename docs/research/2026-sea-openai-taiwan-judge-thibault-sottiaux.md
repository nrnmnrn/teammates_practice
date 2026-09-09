# 2026 Sea × OpenAI 台灣站：Thibault Sottiaux 評審公開訊號

研究日期：2026-09-09
資訊分類：本檔區分可直接核對的職務／公開發言、有限研究推論與待確認事項。它不預測 Thibault Sottiaux 本場評分、權重、投票或產品決策。

## 研究問題

對 Sea × OpenAI 台灣站官方列名評審 Thibault Sottiaux，其公開可驗證工作／發言，對 problem framing（問題框架）、build quality（構建品質）、depth of thought（思考深度）、Codex usage（Codex 應用程度）四項準則，僅支持哪些展示訊號？

## 基本資料

| 欄位 | 可查內容與限制 |
| --- | --- |
| 活動列名 | [台灣站官方活動頁](https://shopee.tw/m/seaxopenai-hackathon-tw)是本任務指定的列名來源。研究日可擷取 HTML 僅含頁名，未能獨立讀到評審名單或四準則；故不由本次擷取自行證實姓名、準則定義或權重。 |
| 活動時點職務 | Sea 的[系列公告](https://www.sea.com/news/395)稱 Thibault Sottiaux 為 Head of Codex at OpenAI。OpenAI Forum 亦稱其為 OpenAI Member of Technical Staff、帶領 Codex。 |
| 職稱時效 | OpenAI 2026-08 的[Ona 公告](https://openai.com/index/openai-to-acquire-ona/)將其署為 Core Products Lead；此可見職稱隨時間改變，本文不把後來職稱回填為活動時點。 |

## 狀態

**案例觀察；完成但證據受限。** 這是評審公開訊號，不是得獎作品或參賽者案例；「已驗證案例」三類門檻不適用，亦未形成跨評審候選或團隊已採納做法。

## 來源

### 官方活動／職務來源

- [2026 Sea x OpenAI Regional Codex Hackathon Taiwan 活動官網](https://shopee.tw/m/seaxopenai-hackathon-tw)，Shopee Taiwan，官方活動頁，查閱 2026-09-09。用途：活動／列名直接 URL。限制：本次可擷取文字不足，不能逐字核對評審名單與四準則。
- [Sea and OpenAI Launch First Regional Codex Hackathon Series in Asia Pacific](https://www.sea.com/news/395)，Sea，官方公告，2026-05-14，查閱 2026-09-09。用途：將 Thibault Sottiaux 列作 Head of Codex at OpenAI，並記錄其對系列的具名引言。
- [Codex is for Everyone: Why Codex Matters Beyond Code](https://forum.openai.com/public/events/codex-is-for-everyone-why-codex-matters-beyond-code-fa40puy7wi?autoRsvp=true)，OpenAI Forum，活動頁，查閱 2026-09-09。用途：列其為 OpenAI Member of Technical Staff、帶領 Codex。

### 本人或所屬機構第一手公開來源

- [Event Replay: Codex is for Everyone: Why Codex Matters Beyond Code](https://forum.openai.com/public/videos/event-replay-codex-is-for-everyone-why-codex-matters-beyond-code-2026-05-13)，OpenAI Forum，具逐字稿公開訪談，2026-05-13，查閱 2026-09-09。用途：Thibault Sottiaux 談任務 context、setup friction、可靠性、迭代、問題定位、architecture、使用者回饋與 Codex 工作方式。
- [OpenAI to acquire Astral](https://openai.com/index/openai-to-acquire-astral/)，OpenAI，官方公告，2026-03-19，查閱 2026-09-09。用途：其具名說明 Codex 應覆蓋完整 software-development lifecycle；公告同時說明 lint、format、type safety 與早期錯誤偵測的工具角色。
- [OpenAI to acquire Ona](https://openai.com/index/openai-to-acquire-ona/)，OpenAI，官方公告，2026-08，查閱 2026-09-09。用途：其具名說明 production workflow 所需 security、control、trust 與 scale；是活動後的公開材料，僅作背景訊號。

### 補充／待核實來源

- 無使用私人通信、私信、未署名社群推測或他賽規則。
- [台灣站評選來源核實](2026-sea-openai-taiwan-judging-source-check.md)記錄同一台灣頁的公開擷取限制；此為本 repo 紀錄，不替代官方來源。

## 已確認資訊

- Sea 官方公告將 Thibault Sottiaux 列為 Head of Codex at OpenAI，並稱 Sea × OpenAI 系列將延伸至台灣；這只確認該系列的公開合作背景，非台灣場評分程序。[Sea 公告](https://www.sea.com/news/395)
- OpenAI Forum 的逐字稿記錄他指出，早期雲端 Codex setup friction 高、模型對長時間任務尚不夠可靠且難迭代，因而調整做法；他也說軟體工作包含 ticket、prioritization、architecture、bug investigation 與判斷是否為使用者問題。[逐字稿](https://forum.openai.com/public/videos/event-replay-codex-is-for-everyone-why-codex-matters-beyond-code-2026-05-13)
- OpenAI 的 Astral 公告稱 Codex 目標是參與 plan changes、modify codebases、run tools、verify results、maintain software；同頁說 uv、Ruff、ty 分別協助環境管理、lint／format、type safety 與早期錯誤發現。此為 OpenAI 對產品方向與工具角色的陳述，不是競賽繳交規則。[Astral 公告](https://openai.com/index/openai-to-acquire-astral/)
- OpenAI 的 Ona 公告引 Thibault Sottiaux 說企業需要可完成實事、同時符合 security 與 control 的 agent；公告具體列出 access scope、credentials、activity log 與 review 等 production concern。此為 enterprise 情境，非對 hackathon 的要求。[Ona 公告](https://openai.com/index/openai-to-acquire-ona/)

## 參考性先例

### 關鍵發言、限制與結果

**來源直接陳述**：Thibault Sottiaux 談及 agent 的任務成功依賴足夠 context，早期做法因 setup friction、可靠性與迭代困難而改變；他也把工作置於問題定位、architecture 與 user feedback 的脈絡。OpenAI 的具名材料則將 Codex 描述為可參與規劃、改碼、跑工具、驗證、維護及受控 deployment 的工作流。[逐字稿](https://forum.openai.com/public/videos/event-replay-codex-is-for-everyone-why-codex-matters-beyond-code-2026-05-13) [Astral 公告](https://openai.com/index/openai-to-acquire-astral/) [Ona 公告](https://openai.com/index/openai-to-acquire-ona/)

**研究推論**：可討論的展示訊號是：把任務 context、驗證、取捨與 Codex 對流程的實際貢獻連成可查鏈。這不表示他會以此評分，四準則亦不由本文定義。

### 四準則的有限展示訊號

#### problem framing

- **來源證據：** 他把工程工作描述為先判斷是否為 user problem、問題在哪裡，並指出 agent 取得相關 context 後更能解題。[逐字稿](https://forum.openai.com/public/videos/event-replay-codex-is-for-everyone-why-codex-matters-beyond-code-2026-05-13)
- **研究推論：** 可討論展示任務背景、使用者痛點、可用資料／限制與可觀察成功條件，讓旁觀者知道 agent 為何做此事。
- **何時值得討論：** 問題可能被誤當成純技術 bug，或輸入資料、使用者目標會改變解法時。
- **未來規劃問題：** 哪些必要 context 可安全公開，且足以使他人理解問題與結果？
- **限制／風險：** 此為產品領導者談一般軟體工作，不是台灣場對 problem framing 的定義、題材或配分。
- **待核實項目：** 台灣站是否規定問題敘述、使用者研究或成功指標的格式。

#### build quality

- **來源證據：** 他公開承認長時間任務的可靠性、setup friction 與迭代曾是限制；Astral 公告把 run tools、verify results、lint、format、type safety 與早期錯誤偵測置於工作流程。[逐字稿](https://forum.openai.com/public/videos/event-replay-codex-is-for-everyone-why-codex-matters-beyond-code-2026-05-13) [Astral 公告](https://openai.com/index/openai-to-acquire-astral/)
- **研究推論：** 可討論展示主要失敗模式、一次可重播驗證或品質檢查，並誠實交代未解限制；不只展示成功畫面。
- **何時值得討論：** agent 要跑多步任務、接外部工具，或 setup／可靠性會明顯改變結果時。
- **未來規劃問題：** 最小但有意義的驗證是什麼？失敗時使用者／評審會看見什麼？
- **限制／風險：** 不支持推論台灣站須有 lint、type checker、CI/CD、production security 或完整測試套件。
- **待核實項目：** 台灣站對可運行性、測試、部署、資料與 demo 備援的正式要求。

#### depth of thought

- **來源證據：** 他談 prioritization、architecture、bug investigation、user feedback；並明說早期方案因 friction 與可靠性不足而被調整。[逐字稿](https://forum.openai.com/public/videos/event-replay-codex-is-for-everyone-why-codex-matters-beyond-code-2026-05-13)
- **研究推論：** 可討論展示一項影響結果的取捨：需要何種 context、何處人為介入、哪些方案因可靠性或成本不採用，以及如何驗證。
- **何時值得討論：** 有多種 agent 架構、資料路徑或自動化程度，且各自風險不同時。
- **未來規劃問題：** 哪個選擇最影響可信度，且能以簡短證據讓外部人核對？
- **限制／風險：** 不代表評審偏好複雜架構、長篇技術說明或公開全部內部推理。
- **待核實項目：** 台灣站對設計文件、取捨說明與評審問答的要求。

#### Codex usage

- **來源證據：** Astral 公告把 Codex 放於 plan、modify、run、verify、maintain 的完整流程；其公開訪談亦描述從 task／repository 出發、產生修改並開 PR 的早期工作形態。[Astral 公告](https://openai.com/index/openai-to-acquire-astral/) [逐字稿](https://forum.openai.com/public/videos/event-replay-codex-is-for-everyone-why-codex-matters-beyond-code-2026-05-13)
- **研究推論：** 可討論展示 Codex 接收的任務與 context、產出（如變更或驗證）、人如何 review，以及這些如何改變可見成果；不以工具名稱代替證據。
- **何時值得討論：** 團隊主張 Codex 對建置、debug、測試或驗證有實質貢獻時。
- **未來規劃問題：** 哪一段使用紀錄／結果可公開且不洩漏私密資料？哪一項人為 review 可證明沒有盲信產出？
- **限制／風險：** 這些是 Codex 產品方向與使用案例，非台灣站要求 agent 自主開 PR、提供 prompt、使用特定介面或達特定時數。
- **待核實項目：** 台灣站對 Codex 使用證明、帳號、資料權限、可用工具與權重的直接規則。

## 不可套用台灣站

- Thibault Sottiaux 的公開產品觀點及 OpenAI enterprise／產品材料，不代表其台灣場個人偏好、評分、投票或權重。
- 雲端 workspace、access scope、credential、activity log、長時間 agent 與 production governance，皆不可自動變成黑客松必備功能或交付。
- 本檔不用新加坡站規則、媒體推測或其他評審資料補足台灣站規則。

## 待確認問題

- 本次官方台灣頁可擷取文字未顯示 Thibault Sottiaux 的評審列名；仍需可檢查的主辦方文字、畫面或公告佐證。
- 四項準則的台灣站正式定義、權重、評審程序、提交格式與展示時間尚未取得。
- 未找到 Thibault Sottiaux 對台灣站評分、同場評審協作或特定作品要求的公開第一手說明；不可補造。
