# 2025 World’s Largest Hackathon presented by Bolt：Knowledge Universe（線上）

研究日期：2026-09-09
資訊分類：賽事與提交關係、作品與成員角色為「已確認資訊」；AI coding、返工、token 消耗與團隊工作方式來自團隊在 Devpost 的第一手自述；由此抽出的做法為「參考性先例」。本案例符合 `2026-09-multi-person-ai-coding-alignment` 納入條件，但不是 Sea × OpenAI 台灣站規則、產品需求或團隊決定。

## 研究問題

Knowledge Universe 是否有足夠第一手證據，說明多人團隊如何分工使用 AI coding、保存中間成果、處理 AI 產碼失敗與時間壓力？

**覆核結論：符合。** Devpost 官方提交頁確認作品屬於 World’s Largest Hackathon presented by Bolt，並列出至少兩名具名成員及不同工作角色；同頁第一人稱敘述明確記錄 Bolt prompting、GitHub rollback 目的、custom project prompt 失敗、token 耗用與末刻改用 Discuss。公開 GitHub repository 與 live demo 提供可檢查作品證據。這足以回答 AI coding、多人責任、返工／整合風險中的多項研究問題；但沒有足夠資料重建同步頻率或完整開發時間表。

## 基本資料

| 欄位 | 可直接查驗的內容 |
| --- | --- |
| 賽事 | World’s Largest Hackathon presented by Bolt；Devpost 賽事頁列為線上、公開活動，由 StackBlitz／Bolt 主辦、Devpost 管理。 [官方賽事頁](https://worldslargesthackathon.devpost.com/) |
| 時間 | 官方賽事頁列 2025-05-30 至 2025-06-30。 [官方賽事頁](https://worldslargesthackathon.devpost.com/) |
| 作品 | Knowledge Universe：將閱讀、電影、遊戲等經驗視為 planets，形成個人 knowledge universe；這是作者提交內容，非本研究獨立功能驗證。 [作品提交頁](https://devpost.com/software/knowledge-universe) |
| AI coding 關聯 | 作者寫明使用 Bolt、only prompting，並列出 JSX、Tailwind CSS、React hooks、Lucide React。 [作品提交頁](https://devpost.com/software/knowledge-universe) |
| 公開成員與角色 | Devpost 列出 Akihiro Sakurai 負責 initial idea／concept-level application generation using Bolt；Masahiro Sakurai 負責 rendering bug fixes；頁面另顯示一位 private user，故公開資料不能確定完整成員總數。 [作品提交頁](https://devpost.com/software/knowledge-universe) |

## 狀態

**案例觀察**。本案為單一外部案例；尚未形成跨案例候選、團隊已採納做法或台灣站規則。

## 來源

### 官方來源

- [World’s Largest Hackathon presented by Bolt 官方 Devpost 賽事頁](https://worldslargesthackathon.devpost.com/)（主辦／賽事平台頁；World’s Largest Hackathon；查閱 2026-09-09）：確認賽事名稱、2025 日期、線上公開形式，以及作品須主要以 Bolt.new 建立等該賽事規則。這些規則不可套用台灣站。
- [Knowledge Universe Devpost 提交頁](https://devpost.com/software/knowledge-universe)（團隊提交；Akihiro Sakurai 等；官方提交平台上的作品頁；查閱 2026-09-09）：確認作品提交至上述賽事、成員角色、AI coding 自述、失敗與取捨記錄，並連出 repository 與 live demo。

### 可檢查作品證據

- [straytec/wormhole GitHub repository](https://github.com/straytec/wormhole)（團隊公開 repository；查閱 2026-09-09）：README 將作品描述為可用經驗建立 Knowledge Universe；公開檔案含 `.bolt`、React／TypeScript source、Supabase migration 與部署設定。
- [GitHub commit history](https://github.com/straytec/wormhole/commits/main)（公開版本歷史；查閱 2026-09-09）：檢視時頁面顯示 61 commits；history 由 `asana17` 與 `manhatsu` 兩個 GitHub identities 共同產生。commit 訊息包含多次 rendering、migration、AuthForm 與 deployment 修正。這支持「有中間成果與反覆修正」；不等於已證明團隊實際執行過一次 rollback。
- [Knowledge Universe live demo](https://enchanting-rabanadas-315130.netlify.app/)（團隊從提交頁連出的 Netlify deployment；查閱 2026-09-09）：公開頁可取得，顯示 Knowledge Universe 入口。研究未登入、註冊或宣稱每項功能仍可用。

### 團隊成員第一手頁／補充來源

- [Akihiro Sakurai（asana17）Devpost profile](https://devpost.com/asana17)（成員本人作品 portfolio；查閱 2026-09-09）：將 Knowledge Universe 列入其唯一一場 hackathon 與作品清單，支持其提交頁身分連結。
- [Akihiro Sakurai（asana17）GitHub profile](https://github.com/asana17)（成員本人公開 profile；查閱 2026-09-09）：公開身份與 repository commit author `asana17` 相符。
- [Masahiro Sakurai 的 Devpost profile link](https://devpost.com/manhatsu)（作品頁連結；查閱 2026-09-09）：作品頁將其標為 rendering bug fixes；查閱時頁面回傳 cache miss，故本案不以該頁補充個人背景。

## 已確認資訊

- Devpost 賽事頁確認 World’s Largest Hackathon presented by Bolt 為 2025-05-30 至 2025-06-30 的線上公開賽事；作品頁把 Knowledge Universe 標為提交至該賽事。 [官方賽事頁](https://worldslargesthackathon.devpost.com/) [作品提交頁](https://devpost.com/software/knowledge-universe)
- 作品頁列出 Akihiro Sakurai 的 initial idea／concept-level application generation using Bolt，以及 Masahiro Sakurai 的 rendering bug fixes。這是公開頁上的角色自述；不能據此推定完整決策權或完整團隊人數。 [作品提交頁](https://devpost.com/software/knowledge-universe)
- 作者明寫「Using bolt, only prompting」，並說明以 JSX、Tailwind CSS classes、React hooks 與 Lucide React 建構作品；這是可追溯的 AI coding 描述，不是因作品含 AI 功能而反推。 [作品提交頁](https://devpost.com/software/knowledge-universe)
- 作者說整合 GitHub 是為了保存 intermediate progress、方便 rollback。這直接證明團隊把版本歷史當作復原手段的設計意圖。 [作品提交頁](https://devpost.com/software/knowledge-universe)
- 作者說 custom project prompt 的 Bolt code generation 表現不佳，特別是 database 的 Row Level Security 使 Supabase 無法正常工作；團隊因此使用過多 Bolt token，最後無法恢復，只能準備 limited application。 [作品提交頁](https://devpost.com/software/knowledge-universe)
- 作者另說 Bolt 有時卡在同一問題且無法修好；團隊嘗試把 coding best practices 寫進 complete prompt，但結果失敗。 [作品提交頁](https://devpost.com/software/knowledge-universe)
- 作者說到最後一刻才注意到 Bolt 的 Discuss 能改善 prompt。來源沒有說明改善幅度、使用次數或是否修復所有功能。 [作品提交頁](https://devpost.com/software/knowledge-universe)
- 公開 repository 存在，並有 source、Supabase migrations、deployment 設定與 commit history；研究查閱時可見 61 commits。這是可檢查作品／版本證據，不是對程式品質或安全性的稽核。 [GitHub repository](https://github.com/straytec/wormhole) [commit history](https://github.com/straytec/wormhole/commits/main)

### 版本回復證據的精確界線

- **來源直接陳述：** 團隊把 GitHub 用於保存中間進度並方便 rollback。 [作品提交頁](https://devpost.com/software/knowledge-universe)
- **本研究檢查：** 公開 history 顯示持續修正，但在查閱到的 commit message 中沒有找到明寫 `revert`、`rollback` 或 `restore` 的提交。
- **因此可說：** 有版本保存／回復策略的第一手描述，且有公開 history 支持其可追蹤性。
- **不可說：** 團隊已在比賽中成功執行某次 rollback，或 rollback 解決了哪個錯誤；来源未提供該事件的 commit、時間或前後差異。

## 參考性先例

### 關鍵決策、限制、取捨與結果

**來源直接陳述：** 團隊以 Bolt prompting 產生初始概念級 application，另由成員處理 rendering bug fixes；以 GitHub 保存中間成果；custom project prompt 導致 Supabase Row Level Security 問題，消耗過多 Bolt token，最終只準備 limited application；完整 best-practices prompt 未奏效，末刻才發現 Discuss 可改善 prompt。 [作品提交頁](https://devpost.com/software/knowledge-universe)

**研究推論：** 多人 AI coding 的責任至少分成「方向／初始生成」與「視覺／渲染修正」兩條線；GitHub history 可作共同狀態，但不能替代人工確認目前版本、資料庫權限與 demo 範圍。當 prompt 反覆失敗時，token budget 與可恢復版本會直接限制可交付範圍。以上是可討論的案例觀察，不是因果證明。

### 候選實務一：以角色邊界接住 AI 初始生成與人工修正

- **來源證據：** Akihiro 自述負責 initial idea／concept-level application generation using Bolt；Masahiro 自述負責 rendering bug fixes。 [作品提交頁](https://devpost.com/software/knowledge-universe)
- **研究推論：** 將「方向與初始產碼」和「具體 bug 修正」指定給可辨識的人，較容易追蹤誰裁決語意、誰驗證畫面；但本案未公開兩人如何同步或交接。
- **何時值得討論：** 團隊同時使用 AI 產生大範圍程式與人工修正局部功能，且需要快速判定誰可改哪些檔案時。
- **未來規劃問題：** 誰負責確認 prompt 對產品意圖的理解？誰擁有 database／permission 與 demo 版本的最終裁決權？
- **限制／風險：** 本案只有作品頁角色自述，沒有 task board、branch policy、review record 或完整責任表；不可推論其分工必然降低衝突。
- **待核實項目：** 未知兩名成員是否各自使用獨立 Bolt session、如何交接背景、是否有固定同步節點。

### 候選實務二：保存中間版本，但把 rollback 視為可驗證能力

- **來源證據：** 團隊說 GitHub 用來保存 intermediate progress、方便 rollback；公開 repository 有 61 commits 與多次功能／migration／deployment 修正。 [作品提交頁](https://devpost.com/software/knowledge-universe) [GitHub repository](https://github.com/straytec/wormhole) [commit history](https://github.com/straytec/wormhole/commits/main)
- **研究推論：** 共同版本歷史可減少 AI 反覆修改造成的不可逆感；但真正可用的 rollback 需明確標記可展示版本、知道如何恢復，以及回復後重跑關鍵驗證。
- **何時值得討論：** AI coding assistant 可能大幅改動多檔案，或 database／外部服務設定失敗成本高時。
- **未來規劃問題：** 哪個 commit 是最後可展示版本？回復後要檢查哪些畫面、權限、部署與資料？誰執行並宣告回復完成？
- **限制／風險：** 本案未公開 rollback commit 或前後差異；GitHub history 的存在不能證明實際回復成功，也不能保證 database state 可回復。
- **待核實項目：** 是否有 tagged checkpoint、branch／owner 規則、migration rollback 計畫與 demo 前固定版本。

### 候選實務三：Prompt 失敗先止損，再調整協作方式

- **來源證據：** custom project prompt 使 RLS／Supabase 工作失敗；團隊耗用過多 Bolt token、只能保留 limited application；完整 best-practices prompt 也失敗，末刻改用 Discuss 改善 prompt。 [作品提交頁](https://devpost.com/software/knowledge-universe)
- **研究推論：** 長 prompt 不必然帶來較好結果；團隊需要把「繼續嘗試、換互動方式、回復舊版本、縮小 demo」視為不同選項，並在 token／時間耗盡前由人裁決。這是反例導出的候選做法，不是作者明示的流程。
- **何時值得討論：** AI coding assistant 重複產生同類錯誤、外部服務權限設定失敗，或 prompt 修改已開始消耗大量額度時。
- **未來規劃問題：** 設定多少次失敗或多少 token 後停止？誰決定改用 Discuss、人工修正或回復？哪些功能可刪除以保住核心 demo？
- **限制／風險：** 作品頁未提供 token 數量、錯誤 log、prompt 原文、Discuss 操作記錄或結果比較，無法量化哪個策略有效。
- **待核實項目：** RLS 問題的精確根因、Discuss 改善的是 prompt 表達還是實際程式、limited application 的具體刪減內容，均未公開。

## 不可套用台灣站

- World’s Largest Hackathon 的 2025 日期、線上形式、Bolt.new 使用要求、提交欄位、獎項與資格限制，皆是該賽事資料；不得當成 Sea × OpenAI 台灣站規則。 [官方賽事頁](https://worldslargesthackathon.devpost.com/)
- Bolt、Supabase、GitHub、Netlify、Discuss 的組合是此團隊的工具與經驗，不是本隊已獲授權的工具清單，也不是台灣站產品要求。
- Knowledge Universe 的角色分工、token 失敗與最後一刻改用 Discuss，不證明同樣人數、時間、模型或 prompt 方式在台灣站會有相同結果。
- 本案沒有公開完整時程、同步頻率、branch policy、prompt 原文、token 數量或成功率；不可把研究推論改寫成固定流程。

## 待確認問題

- 完整成員總數不明：作品頁列出兩名具名成員，另顯示一位 private user；未公開帳號的角色與貢獻不可核實。 [作品提交頁](https://devpost.com/software/knowledge-universe)
- 未知是否真的執行過 rollback：作者描述其目的，但公開 history 沒有明寫 rollback／revert 的 commit；需作者提供 checkpoint、commit 或前後差異才可确认。
- 未知多人協作節奏：沒有公開同步時間、交接格式、共同詞彙、整合責任、demo freeze 或 rehearsal 記錄。
- 未知 token 耗用量與失敗成本：只有「used too much Bolt token」的定性說法，沒有數字、額度規則或操作紀錄。
- 未知 Discuss 的實際效果：作者說末刻發現能改善 prompt，但沒有提供改善前後 prompt、錯誤或功能差異。
- live demo 可能下線或改版；本研究只確認公開頁可取得，不對功能、安全性、資料庫權限或部署持續性作保證。 [live demo](https://enchanting-rabanadas-315130.netlify.app/)
- Sea × OpenAI 台灣站是否允許或要求特定 AI coding tool、GitHub／Netlify、外部資料庫、公開 repository、demo 形式與資料處理方式，仍須主辦方直接確認。

## 已驗證案例門檻判定

**符合 `2026-09-multi-person-ai-coding-alignment` 的納入條件。**

1. **賽事／提交關係：** 官方 Devpost 賽事頁確認 World’s Largest Hackathon presented by Bolt；作品提交頁明示 Knowledge Universe submitted to 該賽事。 [官方賽事頁](https://worldslargesthackathon.devpost.com/) [作品提交頁](https://devpost.com/software/knowledge-universe)
2. **多人與角色：** 作品頁列出至少兩名具名成員及不同角色：Bolt 初始概念／生成、rendering bug fixes；另明示團隊討論。 [作品提交頁](https://devpost.com/software/knowledge-universe)
3. **AI coding 與第一手經驗：** 作品作者第一人稱記錄 Bolt prompting、custom prompt failure、RLS／Supabase 問題、token 耗用、完整 prompt 失敗及末刻改用 Discuss。 [作品提交頁](https://devpost.com/software/knowledge-universe)
4. **可檢查作品：** 公開 GitHub repository、commit history 與 live Netlify demo 均由提交頁連出。 [GitHub repository](https://github.com/straytec/wormhole) [commit history](https://github.com/straytec/wormhole/commits/main) [live demo](https://enchanting-rabanadas-315130.netlify.app/)

本判定只表示單案例具備足夠證據成為「案例觀察」。未達跨案例門檻，不構成團隊採納，也不支持台灣站規則或產品決策。
