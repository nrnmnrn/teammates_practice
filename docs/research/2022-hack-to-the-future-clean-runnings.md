# 2022 Hack to the Future：Team Burnout／Clean Runnings（倫敦）

研究日期：2026-09-09
資訊分類：本檔將 Planes 的賽事／訪談敘述與團隊公開作品標為「已確認資訊」；由此整理的做法皆為「參考性先例」；未知者列「待確認問題」。本案絕非 Sea × OpenAI 台灣站規則、產品需求或團隊決定。

## 研究問題

在兩日、五名初階開發者且互不相識的 Hack to the Future 中，Team Burnout 如何分工、收斂範圍並完成 Demo／Pitch？何種證據支持其 API 失敗與時間壓力的反例？

## 基本資料

| 欄位 | 可直接查驗內容 |
| --- | --- |
| 賽事 | Planes 主辦、London 現場 Hack to the Future；官方頁列 2022-07-02 至 03、每日 09:00–18:00，題目是用指定 APIs 做具正向社會或環境影響的數位產品。此乃該場資訊。[官方賽事頁](https://www.planes.studio/learn/hack-to-the-future-july-2022) |
| 隊伍 | 官方訪談列 Michael A、Antony Long、Elena Marinaki、Adrian Hards、Sandra Skolarczyk 五人；皆為 junior developers，部分約六個月經驗。[官方團隊訪談](https://www.planes.studio/learn/meet-the-hackathon-team-team-burnout) |
| 作品 | Clean Runnings 讓跑者標示路線並查看沿途空氣品質；團隊 README 稱其以 Google JavaScript API 與 Ambee API 實作。[官方團隊訪談](https://www.planes.studio/learn/meet-the-hackathon-team-team-burnout) [團隊 repo](https://github.com/sandiskolarczyk/clean-runnings) |
| 成果 | Planes 訪談稱隊伍獲 `Product Pitch`（demo 有實際跑步）及 `Innovation`（善用 APIs）類別；此為主辦方敘述，非因果證明。[官方團隊訪談](https://www.planes.studio/learn/meet-the-hackathon-team-team-burnout) |

## 狀態

**案例觀察**。單一外部案例，不構成跨案例候選、團隊已採納做法或台灣站任何規則。

## 來源

### 官方來源

- [Hack to the Future: 2–3 July 2022](https://www.planes.studio/learn/hack-to-the-future-july-2022)，Planes Studio，主辦方賽事頁，查閱 2026-09-09。支持賽期、現場形式、指定 APIs、mentor 配對與活動定位；不支持台灣站規則。
- [Meet the hackathon team: Team Burnout](https://www.planes.studio/learn/meet-the-hackathon-team-team-burnout)，Kate Westbrook／Planes Studio，主辦方團隊訪談，查閱 2026-09-09。支持隊員自述、成果與主辦方對獎項的敘述；隊員經驗仍屬受訪者第一手回顧。

### 可檢查作品證據

- [Clean Runnings 作品頁](https://sandiskolarczyk.github.io/clean-runnings/)，Team Burnout／Sandra Skolarczyk GitHub Pages，公開作品頁，查閱 2026-09-09。HTTP 擷取僅得題名，不能據此驗證現時功能。
- [clean-runnings repository](https://github.com/sandiskolarczyk/clean-runnings)，Team Burnout 貢獻者，公開 source repository／README，查閱 2026-09-09。README 說明問題、技術堆疊與 demo；非獨立功能、安全或效能驗證。
- [Map API 失敗與修復前後 commits](https://github.com/sandiskolarczyk/clean-runnings/commits/main)，Team Burnout 貢獻者，公開版本紀錄，查閱 2026-09-09。2022-07-02 有 `map not rendering`、`API key bad`、`Add a marker to the map`；2022-07-03 有合併 Google Maps／Ambee API。commit 訊息是開發痕跡，不足以量化修復品質或所有人的工作量。

### 補充來源／待核實來源

- 無。本研究未以新聞、轉貼或搜尋摘要確認事實。

## 已確認資訊

- **賽事直接陳述：** Planes 說明本場為兩日現場活動，並為每隊配對 development、design、product mentor；題目要求利用一組 APIs 做有社會／環境正面影響的數位產品。[官方賽事頁](https://www.planes.studio/learn/hack-to-the-future-july-2022)
- **隊伍直接陳述：** 官方訪談的問題設定是：用 sustainable APIs 與 brief，和陌生人於兩日內做前端 React app。Michael 回顧他們很快分配並自選任務；Elena 回顧大隊伍短時程的協調是學習；Adrian 描述與較熟悉技術棧者 pair programming。[官方團隊訪談](https://www.planes.studio/learn/meet-the-hackathon-team-team-burnout)
- **收斂直接陳述：** Antony 說因時間必須犧牲欲做功能；Sandra 說需在 deadline 下調低雄心並同意優先序；Michael 問 MVP 是否可在時間內完成。此支持「曾明確談優先序與 MVP」，不支持已知完整 feature cut list 或決策順序。[官方團隊訪談](https://www.planes.studio/learn/meet-the-hackathon-team-team-burnout)
- **API 反例直接陳述：** Adrian 說第一日結束時 Map API 尚未運作，隊伍幾乎刪除路線 marker；訪談稱後來以深夜 coding 與 Stack Overflow 解決。repo 同日 commit 亦記 `map not rendering`、`API key bad`，後續才記 marker。故 API 整合確曾威脅核心路線功能；**不可**將深夜工作當可複製或健康的成功條件。[官方團隊訪談](https://www.planes.studio/learn/meet-the-hackathon-team-team-burnout) [版本紀錄](https://github.com/sandiskolarczyk/clean-runnings/commits/main)
- **Demo／Pitch 直接陳述：** Planes 說其 `Product Pitch` 獲獎理由是 demo「going the extra mile」，包含實際跑步；另稱 API 使用獲 `Innovation`。這支持 demo 有身體化／情境化呈現及官方結果；不支持「實跑」造成得獎，亦未公開 pitch 腳本、時長、評分表或同組比較。[官方團隊訪談](https://www.planes.studio/learn/meet-the-hackathon-team-team-burnout)
- **作品直接陳述：** 團隊 README 將核心價值限定為：顯示自訂跑步路線沿線 AQI（Air Quality Index，空氣品質指數），並列 React、styled-components、Google JavaScript API 與 Ambee API。這是作者的作品說明，不表示研究已重跑或現場 service 仍可用。[團隊 repo](https://github.com/sandiskolarczyk/clean-runnings)

## 參考性先例

### 關鍵決策、限制、取捨與結果

**來源直接陳述：** 團隊先快速分配任務；在時間不足時討論優先序與 MVP，並準備犧牲功能。Map API 延至首日末仍不能用，使 marker 一度面臨刪除；最後仍保留。公開成果是兩項類別獎，且其中一項由主辦方描述為有跑步的 demo。[官方團隊訪談](https://www.planes.studio/learn/meet-the-hackathon-team-team-burnout)

**研究推論：** 此單案較支持「早分任務、將核心可演示流程與風險整合一起評估、遇阻時重新判定保留或刪除」；不支持「熬夜可解決 API 風險」或「情境化演出必然得獎」。沒有公開資料說明由誰 owner 哪一項任務、優先序如何決定、何時凍結 scope，或 API 失敗如何正式降級。

### 候選實務一：先對齊一條 MVP 可驗證流程，再分配可獨立交付工作

- **來源證據：** Michael 說團隊很快分工；同時他們共同問 MVP 能否在期限完成。作品說明收斂為「自訂跑步路線沿線 AQI」。[官方團隊訪談](https://www.planes.studio/learn/meet-the-hackathon-team-team-burnout) [團隊 repo](https://github.com/sandiskolarczyk/clean-runnings)
- **研究推論：** 未來若規則已確認，團隊可先寫出一條輸入至可觀察結果的 MVP 流程與完成條件，再把能獨立驗證的工作分派；此僅供討論，非既定分工。
- **何時值得討論：** 題目、可用資料／API、團隊人數與 demo 格式已由本場主辦方確認後。
- **未來規劃問題：** 核心流程的輸入、結果、owner、驗收條件各為何？何時必須停下擴充？
- **限制／風險：** 本案沒有任務看板、角色表、時間線或對照組；不能推出此分工比其他方法快或好。
- **待核實項目：** 五人實際工作分配、交接方式、決策者與測試／整合節點均未公開。

### 候選實務二：預先指定高風險 API 的停止／降級判定，不把加班當 fallback

- **來源證據：** Map API 至首日末未運作，marker 幾乎被刪；repo 留有 `map not rendering`、`API key bad` 與後續 marker commit。團隊最後以深夜工作處理。[官方團隊訪談](https://www.planes.studio/learn/meet-the-hackathon-team-team-burnout) [版本紀錄](https://github.com/sandiskolarczyk/clean-runnings/commits/main)
- **研究推論：** 若 MVP 依賴外部 API，可在開發初期驗證它，並在預定時間點決定保留、替代或移除，不將深夜搶修視為標準解方；此為風險降低候選，不是本案已證明的流程。
- **何時值得討論：** 一個第三方 API、帳號、網路或資料品質失敗即可使核心 demo 無法完成時。
- **未來規劃問題：** 哪項失敗會阻斷核心展示？截止時刻與可誠實展示的替代流程為何？誰能決定停損？
- **限制／風險：** 本案最終修復不表示同類 API 故障皆可修復；commit 訊息也不能證明 API key 的真正原因、修復穩定度或健康代價。
- **待核實項目：** 未公開 API SLA、測試結果、故障時間、降級版本或是否曾於 demo 再失敗。

### 候選實務三：Demo 以使用情境連結核心流程，並把 pitch 主張限於可驗證內容

- **來源證據：** 官方稱 Clean Runnings 幫跑者找較乾淨路線；`Product Pitch` demo 有實際跑步，並獲該類別獎。README 對應地描述自訂路線沿線 AQI。[官方團隊訪談](https://www.planes.studio/learn/meet-the-hackathon-team-team-burnout) [團隊 repo](https://github.com/sandiskolarczyk/clean-runnings)
- **研究推論：** 未來可討論令 demo 的真實使用情境與一條可檢查的核心流程對齊；pitch 可說明輸入、結果與已知限制。不能從本案推論要採實體表演或相同題材。
- **何時值得討論：** 核心流程已穩定可重播，且主辦方確認 demo／pitch 的時間、格式與設備後。
- **未來規劃問題：** 觀者可在多短時間內看懂誰遇到何問題、系統做了什麼、結果如何驗證？外部服務失敗時說明何限制？
- **限制／風險：** 「going the extra mile」沒有公開定義；不知 demo 時長、評審標準、其他隊表現，故不能將結果歸因於跑步演出。
- **待核實項目：** 原始 pitch、錄影、評審評語、評分權重及提交版本均未找到。

## 不可套用台灣站

- 本場的倫敦現場兩日 09:00–18:00、junior audience、指定 sustainable APIs、mentor 配對、React 前端 brief、獎項類別與用品安排，全為 Hack to the Future 資訊，不是 Sea × OpenAI 台灣站已確認規則。[官方賽事頁](https://www.planes.studio/learn/hack-to-the-future-july-2022)
- Team Burnout 的五人組成、Google Maps／Ambee、實跑 demo、late-night coding 與 Stack Overflow，皆非本隊工具要求、健康工作規範或產品決策。
- 此案為一個有結果的個案，不能證明所述分工、刪減、API 選擇或 demo 形式造成獲獎。

## 待確認問題

- `https://sandiskolarczyk.github.io/` 根 URL 在研究日 HTTP `404`；訪談鏈向 `https://sandiskolarczyk.github.io/clean-runnings/`，但 HTTP 擷取僅得題名。故不宣稱 live demo 目前可用，也未執行任何功能。
- Planes 頁現標示 2025 發布日期，雖題名與內文指向 2022 活動；本研究未取得 2022 原始規則、完整 judging rubric、pitch 時長、提交要求或參賽名單。
- 沒有公開的任務分配紀錄、scope 清單、MVP 定義、API key 故障根因、測試、失敗 demo、完整 demo／pitch 錄影或評審回饋。
- Sea × OpenAI 台灣站的題目、時程、隊伍規模、API／外部服務限制、demo／pitch 規格、評分與設備仍待台灣站主辦方直接確認。

## 已驗證案例門檻判定

**符合，但證據受限。**

1. **主辦方直接成果證據：** Planes 官方訪談明述 Team Burnout 參加該場並獲 `Product Pitch` 與 `Innovation`。[官方團隊訪談](https://www.planes.studio/learn/meet-the-hackathon-team-team-burnout)
2. **可檢查作品證據：** 團隊 GitHub repo 有 README、程式與版本紀錄；作品 URL 亦由主辦方訪談連出，但本研究只可讀到其題名。[團隊 repo](https://github.com/sandiskolarczyk/clean-runnings) [作品頁](https://sandiskolarczyk.github.io/clean-runnings/)
3. **團隊第一手紀錄：** Planes 具名訪談保留多位隊員對分工、取捨、MVP、API 阻礙與 demo 的第一手回答；repo commit 另留下同期開發痕跡。[官方團隊訪談](https://www.planes.studio/learn/meet-the-hackathon-team-team-burnout) [版本紀錄](https://github.com/sandiskolarczyk/clean-runnings/commits/main)

這只表示三類證據可回查；它不是跨案例門檻，亦未授權本隊採納任何候選。
