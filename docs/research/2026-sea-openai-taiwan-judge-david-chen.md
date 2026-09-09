# 2026 Sea × OpenAI 台灣站：David Chen 評審公開訊號

研究日期：2026-09-09
資訊分類：本檔分開記錄可直接核對的身分／公開發言、由其發言導出的有限展示訊號，以及待確認事項。這不是 David Chen 本場的評分偏好、投票、權重或產品決策。

## 研究問題

對 Sea × OpenAI 台灣站官方列名評審 David Chen，其公開可驗證工作／發言，對題設四項準則——problem framing（問題框架）、build quality（構建品質）、depth of thought（思考深度）、Codex usage（Codex 應用程度）——僅支持哪些展示訊號？

## 基本資料

| 欄位 | 可查內容與限制 |
| --- | --- |
| 活動列名 | [台灣站官方活動頁](https://shopee.tw/m/seaxopenai-hackathon-tw)是本任務指定的官方列名來源。研究日可擷取 HTML 僅含頁名，未能獨立讀到評審名單或四準則文字；因此不以本次擷取結果自行證實姓名、準則定義或權重。 |
| 現職 | Sea 的[管理團隊頁](https://www.sea.com/aboutus/leadership/management/DavidChen)列 David Chen 為 Sea 共同創辦人及 Shopee Chief Product Officer。 |
| 公開一手材料 | OpenAI 的[訪談頁](https://openai.com/index/sea-david-chen/)明示受訪者為 David Chen，並載其對 Sea 使用 Codex、工程流程與區域 hackathon 的公開說法；此為具署名歸屬的公開訪談，不是本場評審指示。 |

## 狀態

**案例觀察；完成但證據受限。** 本案是評審公開訊號，不是得獎作品或參賽者案例，故「已驗證案例」三類門檻不適用，也不形成跨評審候選或團隊已採納做法。

## 來源

### 官方活動／職務來源

- [2026 Sea x OpenAI Regional Codex Hackathon Taiwan 活動官網](https://shopee.tw/m/seaxopenai-hackathon-tw)，Shopee Taiwan，官方活動頁，查閱 2026-09-09。用途：活動／列名的直接 URL。限制：可擷取文字不足，無法由本次公開擷取逐字驗證 David Chen 與四項準則。
- [Sea Leadership：David Chen](https://www.sea.com/aboutus/leadership/management/DavidChen)，Sea，官方管理團隊頁，查閱 2026-09-09。用途：職務與共同創辦人身分。

### 本人或所屬機構第一手公開來源

- [Sea's View on the Future of Agentic Software Development with Codex](https://openai.com/index/sea-david-chen/)，OpenAI 對 David Chen 的具署名訪談，2026-05-14，查閱 2026-09-09。用途：他的公開看法：理解產品需求、trace dependencies、處理 edge cases、test-driven implementation、test coverage、resilient systems，以及 Codex 從 autocomplete 走向 agentic workflow。
- [Sea and OpenAI Launch First Regional Codex Hackathon Series in Asia Pacific](https://www.sea.com/news/395)，Sea，官方公告，2026-05-14，查閱 2026-09-09。用途：David Chen 具名談及實作回應真實世界需求；此為組織公告中的歸屬引言，不是評分準則。

### 補充／待核實來源

- 無使用私人通信、私信或未署名社群推測。
- [台灣站評選來源核實](2026-sea-openai-taiwan-judging-source-check.md)記錄同一官方頁的公開擷取限制；它是本 repo 的研究紀錄，不替代官方來源。

## 已確認資訊

- Sea 官方管理頁列 David Chen 為共同創辦人與 Shopee Chief Product Officer。[Sea Leadership](https://www.sea.com/aboutus/leadership/management/DavidChen)
- OpenAI 訪談將 David Chen標作 Sea 共同創辦人及 Shopee CPO，並記載他認為工程重點不只寫 syntax，還包括依賴關係、legacy logic 與 peak load 下的可靠性；此為他在 Sea 規模與情境下的公開觀點。[OpenAI 訪談](https://openai.com/index/sea-david-chen/)
- 同一訪談記載其對 Codex 的描述：可協助理解產品需求、提出 test-driven implementation、找 distributed system 的 edge cases、加速 debug，並以 alternative implementation 與 test coverage 支援更 resilient 的系統。[OpenAI 訪談](https://openai.com/index/sea-david-chen/)
- Sea 官方公告歸屬 David Chen 的說法為：讓 builder 實作回應 real-world needs 的解法；這確認公開活動方向，非本場評分細則。[Sea 公告](https://www.sea.com/news/395)

## 參考性先例

### 關鍵發言、限制與結果

**來源直接陳述**：David Chen 對 Codex 的公開描述著重於：由產品需求出發、檢查依賴／舊邏輯／可靠性、以 test-driven implementation 與 edge cases 驗證、再以 AI 協助 prototype、debug 與 test coverage。Sea 公告另引他說明 AI 應回應真實世界需求。[OpenAI 訪談](https://openai.com/index/sea-david-chen/) [Sea 公告](https://www.sea.com/news/395)

**研究推論**：若只把公開發言當作「可討論展示訊號」，較可觀察的是問題與使用者的連結、可見的品質證據、對限制與替代方案的說明、以及可檢查的 Codex 工作過程。這不表示他會以此打分、四項準則等重，或只接受這種展示。

### 四準則的有限展示訊號

#### problem framing

- **來源證據：** 他談及 AI 應實作回應 real-world needs 的解法，並把產品需求列為 agent 可推理的輸入。[Sea 公告](https://www.sea.com/news/395) [OpenAI 訪談](https://openai.com/index/sea-david-chen/)
- **研究推論：** 可討論展示「哪個使用者在何種情境遭遇何種具體問題」，並使 demo 的輸入、限制與預期結果對應該問題。
- **何時值得討論：** 團隊已有候選問題，需判斷 demo 是否只展示功能，或已說清真實使用情境。
- **未來規劃問題：** 哪一項可觀察結果能證明作品處理所述問題，而非只產生看似合理的輸出？
- **限制／風險：** 發言不定義本場的 problem framing，也未指定題材、使用者、證據格式或分數。
- **待核實項目：** 台灣站是否公開定義此準則及其權重。

#### build quality

- **來源證據：** 他談及 dependencies、legacy logic、peak-load reliability、test coverage、technical debt 與 resilient systems。[OpenAI 訪談](https://openai.com/index/sea-david-chen/)
- **研究推論：** 可討論以 demo 呈現一項主要失敗情境、測試／驗證結果或明確限制，使作品品質不只停留在 happy path（僅成功路徑）。
- **何時值得討論：** 作品要接觸外部服務、資料或多步流程，且失敗會改變展示結論時。
- **未來規劃問題：** 哪個最可能失敗的情境可被安全、誠實且重播地展示？
- **限制／風險：** 這是大型 Sea 系統的工程觀點，不能推出 hackathon 必須提供 production-grade（正式上線等級）可靠性、完整 CI/CD 或全部測試。
- **待核實項目：** 台灣站對測試、部署、資料、可靠性或可重播 demo 是否另有要求。

#### depth of thought

- **來源證據：** 他指出工程摩擦包含追蹤依賴、理解舊邏輯；也談到 alternative implementations、distributed-system edge cases、system design 與 product judgment。[OpenAI 訪談](https://openai.com/index/sea-david-chen/)
- **研究推論：** 可討論展示關鍵取捨：選擇何方案、排除何替代方案、已知邊界條件為何，及其理由。
- **何時值得討論：** 有兩種以上合理架構、資料來源或 agent 行為，且選擇會影響使用者風險或結果時。
- **未來規劃問題：** 哪一項取捨最能改變結果，且能以短版證據讓旁觀者核對？
- **限制／風險：** 公開訪談不等同他要求參賽者披露完整架構、內部推理或所有方案；亦不代表「複雜」會得高分。
- **待核實項目：** 台灣站是否定義 depth of thought 所需的提交內容或判讀方式。

#### Codex usage

- **來源證據：** 他明說 Sea 將 Codex 從 passive autocomplete 轉向 integrated agentic workflow，並提及需求推理、test-driven implementation、edge-case 發現與 debug。[OpenAI 訪談](https://openai.com/index/sea-david-chen/)
- **研究推論：** 可討論展示 Codex 在一個具體步驟的角色、產出如何被人檢查，以及該產出如何進入可見結果；不把「使用過 Codex」當成唯一證據。
- **何時值得討論：** 團隊欲主張 Codex 帶來可觀察的開發、驗證或探索價值時。
- **未來規劃問題：** 可公開、可重現且不洩漏私密資料的 Codex 使用證據是什麼？人如何檢查其產出？
- **限制／風險：** 此訪談談 Sea 內部流程，不能推為台灣站要求 agentic workflow、CI/CD、prompt 紀錄或特定使用時數。
- **待核實項目：** 台灣站對 Codex 使用證明、可用工具、帳號、資料及評分權重的直接規則。

## 不可套用台灣站

- David Chen 的現職與過去公開發言只提供參考性訊號，不能推論其台灣站個人偏好、評分、投票或權重。
- Sea 的大型 microservices、CI/CD、technical debt 與內部 Codex rollout 情境，不是參賽隊必備架構或交付要求。
- 本檔不把新加坡站規則、得獎結果或媒體報導當成台灣站規則。

## 待確認問題

- 台灣官方活動頁在本次可擷取內容中未能顯示 David Chen 的評審列名；仍需主辦方可檢查的文字、畫面或公開公告佐證。
- 四項準則的台灣站官方定義、權重、評審流程、提交格式與展示時間，均未由本檔推定。
- 未找到 David Chen 對台灣站評分、其他評審協作或特定作品要求的公開第一手說明；缺少這些資料時，不可補造。
