# 多人 AI coding 的責任、節奏與語意對齊

## 研究契約

- **批次 ID：** `2026-09-multi-person-ai-coding-alignment`
- **日期：** 2026-09-09
- **核心研究問題：** 小型黑客松團隊如何在有限時間內並行使用 AI 產生或修改程式，同時維持單一產品意圖、清楚責任、可整合成果與可演練 Demo？
- **適用範圍：** 有兩名以上參與者、公開可核實參賽身分或成果，且有第一手資料可追溯 AI coding、多人協作、返工、整合或交付節奏的黑客松案例。
- **目標：** 取得三個已驗證案例，優先涵蓋至少兩場競賽；最多篩選八個候選案例。
- **既有研究邊界：** `2026-09-team-flow-demo-pitch.md` 已研究一般分工、收斂與 Demo；本批只補足多人 AI coding 的背景共享、責任邊界、整合節奏與語意偏移證據，不重述一般黑客松時間管理建議。

### 名詞定義

- **AI coding：** 參賽者可追溯地描述以 AI 產生、修改、除錯或驗證程式的行為。本文不因作品含 AI 功能，便推定團隊用 AI 寫程式。
- **vibe coding：** 本批僅納入參賽者可追溯地描述以自然語言驅動 AI 產碼或修改，以及其檢查、返工或整合經驗；不用研究者觀察作品後自行推測。
- **語意偏移：** 多人或多個 AI 工作階段對同一目標、詞義、責任、介面或驗收結果形成不一致理解，且來源顯示此差異造成返工、衝突、整合風險，或需額外對齊。若來源未記錄影響，只能列為可能風險。
- **責任：** 某項成果、決策或整合失敗有可辨識的負責人或裁決方式；不等同固定職稱。
- **時間分配：** 有來源支持的階段、檢查點、凍結點、同步頻率或取捨時點；不把研究者自訂比例寫成案例事實。

## 納入與排除

### 納入條件

- 可由主辦方、官方成果頁或等效直接來源確認事件與團隊／作品關係。
- 有可檢查的提交、程式庫、Demo 或其他作品證據。
- 至少一份團隊成員第一手公開紀錄，能回答 AI coding 與多人協作、返工、整合或時間節奏中的至少兩項。
- 證據足以區分來源直接陳述、研究推論與未知事項。

### 排除條件

- 純通用團隊管理文章，未連結至可核實黑客松案例。
- 僅單人參賽，或只證明作品使用 AI 功能，未證明使用 AI coding。
- 只有宣傳摘要、搜尋片段或第三方轉述，無法核實參賽關係與第一手工作經驗。
- 只有最終成果，無責任、節奏、對齊、返工或整合資料。

## 共同比較問題

1. 團隊如何界定產品意圖、共同詞彙與「完成」？
2. 誰負責方向裁決、分支／檔案、整合、驗證與 Demo；責任是否隨階段改變？
3. 團隊如何把必要背景交給其他成員及其 AI 工作階段？
4. 團隊何時同步、整合、凍結與演練；來源能支持哪些實際時點或順序？
5. 哪些語意偏移、程式衝突、AI 錯誤或返工確實發生；團隊如何發現與處理？
6. 哪些做法未奏效、被放棄，或只在特定人數、賽制、工具與時間限制下成立？

## 來源要求、輸出與停止條件

- 每案至少保留：一份官方／提交證據、一份可檢查作品證據、一份團隊成員第一手公開紀錄。單一頁面若同時具兩種角色，仍須說明其證據限制。
- 優先第一手賽後回顧、公開開發紀錄、提交頁、程式庫歷史與 Demo；第三方整理只作線索或補充。
- 主動搜尋返工、整合失敗、工具棄用、未完成與失敗 Demo；未找到反例，不推論反例不存在。
- 每個入選案例各寫一份 `docs/research/YYYY-事件-作品.md`；本檔最後補入候選池、比較、跨案例候選、反例、限制及待確認問題，再更新 `docs/research/sea-openai-winning-patterns.md`。
- 三個已驗證案例後停止擴張；若篩選八案仍只有兩案合格，標為「完成但證據受限」；一案或零案則標為「研究不完整」，不提出跨案例候選。
- 不保存來源全文、私人通信、個人資料或秘密；不修改 `team-playbook/`、`build-handoff/`、`TODO.md`、`hackathon-prd.md` 或 `tmp_idea.md`。

## 台灣站不可套用界線

其他競賽案例一律為參考性先例，不得據此宣稱 Sea × OpenAI 台灣站的隊伍人數、時長、工具、網路、提交方式、評分或 AI coding 規則。主辦方尚未直接確認者皆列待確認問題；本批結果亦不自動成為本隊現場規則、產品需求或實作決策。

## 候選池與篩選記錄

下表只記錄查證入口與暫定判斷，不等同正式案例或已確認資訊。

| 候選 | 事件／作品 | 初步線索 | 暫定判斷 |
| --- | --- | --- | --- |
| 1 | Recife 公立大學一日 vibe coding hackathon | [ICSE SEET 2026 論文](https://arxiv.org/abs/2512.02750)記錄九組混合經驗團隊、協作、工具流程與返工 | 納入；已驗證，作品重現證據受限 |
| 2 | Lisbon 三日 AI audio hack event | [活動觀察者第一手復盤](https://schristoph.online/blog/ai-hackathon-observations/)稱七隊並行、整合成單一產品，並比較 vibe coding 與規格驅動邊界 | 待補直接查證；不計入已驗證案例與事件總數 |
| 3 | World’s Largest Hackathon／Veritas | [Devpost 提交頁](https://devpost.com/software/veritas-4lmbop)列多人、分階段使用 Bolt.new 與 Cursor、整合及環境失敗 | 納入；已驗證，責任與版本歷史仍未知 |
| 4 | World’s Largest Hackathon／Knowledge Universe | [Devpost 提交頁](https://devpost.com/software/knowledge-universe)列多人角色、GitHub rollback、完整 prompt 失敗與末刻修正 | 納入；已驗證，與候選 3 並行完成後即停止擴張 |
| 5 | STRIDE 2025／8-Hour Hackathon Hustle | [B.M.S. College of Engineering 公開報告](https://bmsce.ac.in/reports/MCA/STRIDE_-2025.pdf)記載團隊使用 Cursor、Git 與 GitHub | 達三案停止條件後不續查；不得計入比較 |
| 6 | World’s Largest Hackathon／Axiom | [Devpost 提交頁](https://devpost.com/software/axiom-51906d)有 AI coding、耗盡額度、重做與除錯紀錄，但只列一位建立者 | 排除：不符合兩名以上參與者條件；只作失敗線索 |
| 7 | World’s Largest Hackathon／StartTeam | [Devpost 提交頁](https://devpost.com/software/start-team)聲稱 GitHub 協作，但只列一位建立者，且欠缺具體返工與責任紀錄 | 排除：團隊關係與工作過程不足 |
| 8 | World’s Largest Hackathon／Examinator | [Devpost 提交頁](https://devpost.com/software/exam-inator)列三位建立者與 48 小時開發，但未交代多人 AI coding 工作流 | 達三案停止條件後不續查；不得計入比較 |

**反例搜尋方向：** AI 產碼造成整合失敗、完整 prompt 反而失效、團隊退回人工修正、版本回滾、額度耗盡、最後凍結或刪減功能。候選 4、6 已有初步失敗線索；仍須逐案查證。未找到失敗紀錄，不代表沒有失敗。

## 入選案例

- [2025 Recife Vibe Hack](../2025-recife-vibe-coding-hackathon.md)：官方活動／成果、研究論文與參賽者第一手紀錄可互證；有 shared prompt、Kanban、shared document、角色分配、多工具流程與人工返工資料。公開 repository、Demo 與逐隊版本歷史未找到。
- [2025 World’s Largest Hackathon／Veritas](../2025-bolt-veritas.md)：官方提交平台、live demo、影片及團隊第一手提交敘述可查；有 Bolt.new、Cursor、Claude 的階段切換、整合與接近提交時的環境除錯資料。無 prompt、branch、commit 或逐人責任紀錄。
- [2025 World’s Largest Hackathon／Knowledge Universe](../2025-bolt-knowledge-universe.md)：官方提交平台、公開 repository、commit history、live demo 與團隊第一手敘述可查；有兩名成員角色、AI prompt 失敗、token 耗用、有限成果與版本復原意圖。未證明實際執行 rollback。

Lisbon 案仍列候選池，待補足事件／團隊關係與第一手工作紀錄後再判定；目前不列入入選案例、跨案例比較或總數。

## 共同比較

| 問題 | Recife | Veritas | Knowledge Universe |
| --- | --- | --- | --- | --- |
| 意圖與完成定義 | 有共同 prompt、shared document 與可展示原型目標；逐隊驗收條件未知 | 以 frontend／core flow、backend／API、database／core functionality 描述階段；正式驗收文件未知 | 初始概念生成與 rendering 修正分開；共同驗收定義未知 |
| 責任分配 | 部分團隊設 leader、lead programmer、designer；亦有鬆散角色 | 至少兩名成員；公開資料未列各階段 owner、merge、QA 或 Demo owner | 一人負責初始構想／Bolt 生成，一人修 rendering bug；完整決策權未知 |
| AI context 共享 | prompt、Kanban、shared document 分配人與 AI 工作 | 工具依階段切換；工具間如何傳遞 context 未公開 | 嘗試 custom／完整 prompt，後改用 Discuss；多人如何共享 prompt 未公開 |
| 時間與整合 | 單日流程由構想、視覺原型、prompt 整理、產碼至修整；無逐隊凍結點 | 團隊自報十二日；三階段切換，接近提交時處理 local server 故障 | 有 61 commits 與末刻調整紀錄；無完整時間表或凍結點 |
| 失敗與返工 | 大而模糊的 prompt、不穩定輸出、credit／token 限制，促成補 prompt 或手改 code | local server 與 configuration conflict 威脅提交；團隊稱除錯後部署 | 完整 prompt 與 database 權限設定失敗、耗用過多 token，只保留 limited application |
| 不可泛化處 | 教學活動、初學者、單日、匿名研究資料 | 一個線上提交、較長活動窗口、團隊自述多、版本證據少 | 同一 Bolt 賽事、團隊自述多；rollback 只是意圖，未證實實行 |

三案皆未公開完整責任矩陣、固定同步頻率、code freeze、Demo rehearsal 或逐時工時。因此本批不能證成「最佳角色表」或固定時間比例。

## 證據不足觀察

### 觀察一（非跨案例候選）：先由人共同寫清意圖與外部邊界，再讓 AI 處理有界工作

- **支持案例：** [Recife](../2025-recife-vibe-coding-hackathon.md)記錄 shared prompt、shared document、角色與人為修整；[Veritas](../2025-bolt-veritas.md)僅提供依技術階段切換工具的部分支持。
- **反例／不支持：** Recife 無公開逐隊 prompt 與 commit；Veritas 無 handoff 文件；Lisbon 尚待查證。現有直接證據不足兩個完整支持案例，故降為研究觀察，不成立跨案例候選。
- **研究推論：** 多人並行前，可討論先共享一份可查的產品意圖、詞義、相依輸入／輸出與完成例；AI 工作階段只取當前任務所需 context。此為候選，非本隊規則。
- **適用條件：** 至少兩項工作互相依賴，且不同人或 AI 工作階段可能各自解讀同一概念。
- **限制：** 來源未直接使用「語意偏移」衡量，也無對照組；只能以模糊 prompt、component mismatch、工具交接等可觀察現象作近似訊號。
- **待確認問題：** 誰裁決共同詞義？哪份文件是唯一有效版本？介面何時允許變更？
- **狀態：** 降級為研究觀察；尚未獲團隊採納。

## 跨案例研究候選

### 責任依交付邊界分配；人持有裁決、整合與驗證

- **支持案例：** [Recife](../2025-recife-vibe-coding-hackathon.md)記錄人員角色、任務工具與人工檢查；[Knowledge Universe](../2025-bolt-knowledge-universe.md)列出初始生成與 rendering 修正的不同人類角色；[Veritas](../2025-bolt-veritas.md)顯示 AI 產碼後仍須人工處理環境與部署問題。
- **反例／不支持：** 三案皆未公開完整 owner 表；Veritas 尤其不能證明其階段等同逐人分工。故不支持照抄固定職稱，亦不支持 AI 可擔任最終責任人。
- **研究推論：** 可討論讓每個可交付邊界各有一名人類負責人，另明示誰裁決方向、誰整合、誰驗證可執行成果；AI 為執行工具，不承擔最終責任。
- **適用條件：** 多人或多工具產出最後須合為同一 Demo。
- **限制：** 案例規模由單隊至七隊不等；最佳 owner 數、能否兼任及輪替方式均無證據。
- **待確認問題：** 本隊人數與技能未定時，哪些責任不可兼任？負責人失聯或工作失敗時如何接手？
- **狀態：** 跨案例候選；尚未獲團隊採納。

## 跨案例風險觀察

### 觀察二（非跨案例候選）：整合、人工返工與可執行檢查會形成時間風險

- **支持案例：** [Recife](../2025-recife-vibe-coding-hackathon.md)記錄 AI 輸出需補 prompt 或手改 code；[Veritas](../2025-bolt-veritas.md)在接近提交時遭遇 local server／configuration 問題；[Knowledge Universe](../2025-bolt-knowledge-universe.md)記錄 prompt／database 問題耗用過多 token，最後只能交 limited application。
- **反例／不支持：** 三案沒有共同時長、逐時記錄或固定凍結點；因此只能記為整合與返工風險，不能推出成功排程範本。
- **研究推論：** 本案不提出「應列為排程工作」或任何時間比例、凍結點、停止加功能規則；相關作法須待直接證據或團隊決策。
- **適用條件：** 作品含多元件、外部 API、database、部署或多種 AI coding tool。
- **限制：** 只支持「需保留工作」，不支持二成、三成或任何固定比例；亦未證明 code freeze 的最佳時點。
- **待確認問題：** 台灣站總時長、提交截止、網路與展示條件為何？何種失敗觸發停止加功能或切換備案？
- **狀態：** 降級為跨案例風險觀察；尚未獲團隊採納。

## 資料偏差與未解問題

- 三案皆由願意公開活動或作品者留下資料，可能低估失敗、衝突與未完成案例。
- Recife 為研究者彙整的匿名資料；Veritas 與 Knowledge Universe 主要由團隊自行填寫提交頁；Lisbon 尚待直接查證。各案證據角色不同，不宜把語句強度直接等同。
- 公開資料偏重工具、成果與技術困難，少有逐人責任、同步次數、prompt 差異、merge conflict、凍結、排練與備案紀錄。
- Knowledge Universe 雖有公開 commit history，但未證明團隊實際執行 rollback；版本紀錄存在不等於復原流程有效。
- 台灣站隊伍人數、決賽時長、題目、帳號、網路、工具、資料、提交與評分規則仍待主辦方直接確認；在此之前不能將研究觀察換算成現場時間表、產品需求或實作決策。

## 執行狀態

- **進度：** 完成但證據受限；八案候選池中三個已驗證案例，涵蓋兩場事件。Lisbon 案待補直接查證，不計入總數。
- **證據狀態：** 保留一項有三案直接支持的跨案例研究候選；候選一降為證據不足觀察，候選三降為跨案例風險觀察。三者皆非台灣站規則、產品需求或實作決策。
