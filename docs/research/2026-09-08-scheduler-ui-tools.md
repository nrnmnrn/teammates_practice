# 排程策略驗證工具：視覺化 UI 工具研究

查證日期：2026-09-08

本筆記只研究展示層工具，不核定產品，也不重新解讀競賽規則。對照的候選是「背景報表工作、固定第 30 單位快照、四列策略比較、兩個處理節點時間線、AI 候選被接受或拒絕」。七小時內的成功條件是可重跑、可解釋、失敗可降級；以下工時都是假設估計，沒有在本機實測。

## 建議先給結論

首選 **Streamlit + Plotly**，前提是至少一人能在現場快速寫 Python，且團隊接受「本機啟動一個 Python web app」的展示方式。Streamlit 原生提供 widgets、layout、表格和多種 chart；`st.plotly_chart` 可直接顯示 Plotly Figure，並支援 selection 事件。官方文件也把 `streamlit run` 作為本機啟動方式，適合將既有 Python 模擬器、評測器和 UI 放在同一個程序中。[Streamlit chart elements](https://docs.streamlit.io/develop/api-reference/charts)、[st.plotly_chart](https://docs.streamlit.io/develop/api-reference/charts/st.plotly_chart)、[Streamlit quick reference](https://docs.streamlit.io/develop/quick-reference/cheat-sheet)

若必須要更精緻的互動時間線、動畫或多人前端協作，選 **React + Recharts**；若圖表本身需要 data zoom、timeline 元件和較多瀏覽器端互動，選 **React + Apache ECharts**。若團隊更在意最快接上 Python 函式和按鈕流程，而不是複雜比較圖，**Gradio** 是備選。Figma 和 v0 適合設計或產生 UI 草稿，不應被當作比賽當天的運行 UI：Figma prototype 是可分享的設計互動流程；v0 會生成程式碼和 UI，但需要網路，仍須把程式整合、測試和準備離線降級。[Figma prototyping](https://help.figma.com/hc/en-us/articles/360040314193-Guide-to-prototyping-in-Figma)、[v0 官方說明](https://vercel.com/docs/v0)、[v0 FAQ（離線限制）](https://v0.dev/docs/faqs)

## 後續團隊選型紀錄（非研究結論）

2026-09-09，團隊決定本候選作品優先採 **Gradio + Plotly**。此決定採納江咏宸（Neo）熟悉 Gradio 的已知條件，以降低七小時內的學習與整合風險；不表示 Gradio 的動畫能力優於其他方案，也不改變本研究原先依公開工具能力得到的 Streamlit + Plotly 首選。

選型的產品範圍與停止線見[候選產品規格](../product/task-scheduler-validation-candidate.md)；Neo 的能力紀錄見[團隊準備狀態](../operations/team-readiness.md)。

## 四個主要方案

| 方案 | 符合本題的地方 | 時間線、同步縮放、動畫 | Python／前後端整合 | 離線本機展示與降級 | 假設工時（小時） |
| --- | --- | --- | --- | --- | --- |
| **Streamlit + Plotly** | Python 中直接組四區畫面；Plotly 支援 hover、選取、縮放，表格和指標容易同頁呈現 | 靜態 numeric horizontal bars 可表達兩節點開始／完成區間；多圖同步縮放或播放動畫要額外 wiring，先不做 | 最省：開賽後模擬器、評測器、AI 提案器若已有 Python 輸出即可直接呼叫；不用另建 API | 本機 `streamlit run` 可展示已保存 JSON；模型不可用時仍顯示基準、搜尋和「AI 不可評估」 | 2–4（假設開賽後已有穩定結果資料結構） |
| **Gradio** | `Blocks`、按鈕、表格、文字輸出和 Python callback 很快；官方 `Plot` 接受 Plotly、Altair、Matplotlib、Bokeh figure。[Gradio Plot](https://gradio.app/main/docs/gradio/plot) | 原生 `LinePlot`／`ScatterPlot`／`BarPlot` 可做比較；官方示例有 select 與 x-limit zoom，但兩策略時間線同步與動畫仍需自訂事件 | Python 直連方便；若日後拆前後端，需另加 API 或客製 component | 可本機啟動並用預先存檔結果；網路或模型失敗可保留靜態表與 plot | 2–4（只做基本 dashboard） |
| **React + Recharts** | React component 化、SVG、宣告式圖表；可把「策略列」和「兩節點 lane」做成可測試元件。官方 API 的 `syncId` 可同步 Tooltip／Brush。[Recharts API](https://recharts.github.io/en-US/api/) | 同 `syncId` 的多圖同步 tooltip／brush 已有能力；動畫有現成設定，但要把模擬事件、播放控制和表格狀態接起來，工作量較高 | 需要開賽後其他工作交付的 Python 模擬器結果（JSON/API）；前後端邊界、啟動指令和錯誤格式都要處理 | 可在預先建置的前端靜態檔旁準備本機 HTTP 服務與固定 JSON；模型失敗時很穩。不能假設雙擊 HTML 就能正常載入資料 | 5–9（假設開賽後另有 React skeleton）；初學者風險較高 |
| **React + Apache ECharts** | 瀏覽器端互動圖表範圍大；官方 option 文件列出 `dataZoom` 與 `timeline` 元件，適合未來做播放／狀態切換。[ECharts option 文件](https://echarts.apache.org/en/option.html) | ECharts 的 `timeline` 是配置／資料狀態切換控制，不是排程區間甘特圖；要畫工作開始／完成區間，仍需 custom series 或自行畫矩形。data zoom、動畫也要把共用時間軸、事件游標和播放控制接起來；七小時只做一條靜態 lane | 需要開賽後其他工作交付的 Python 模擬器結果（JSON/API）；ECharts option 與資料轉換比 Plotly 直接傳 Figure 更繁瑣 | 可在預先建置的前端靜態檔旁準備本機 HTTP 服務與固定 JSON；外部字型／CDN 會破壞離線，模型失敗時顯示快照和結果表 | 6–10（假設開賽後另有 React skeleton）；若無前端經驗不建議 |

授權若需列入提交檢查：Recharts repository 標示 MIT；Apache ECharts repository 標示 Apache-2.0。[Recharts LICENSE](https://github.com/recharts/recharts)、[ECharts LICENSE](https://github.com/apache/echarts/blob/master/LICENSE)。這不是法律意見；實作前仍應確認 lockfile 中的相依套件。

### 為何時間線不能直接「畫一畫」

本題的時間是 0–90 的模擬數值，不是日期時間。最小可靠做法是把每個事件轉成 `(strategy, node, start, duration, job_id, status)`，用 Plotly horizontal bar 或 React 的矩形 lane 畫出開始到完成；Plotly 官方支援 horizontal bar 的 `orientation="h"`，也支援用 `base` 指定 bar 起點，可直接表達 numeric interval。[Plotly horizontal bar](https://plotly.com/python/horizontal-bar-charts/)、[Plotly Bar `base`／`orientation`](https://plotly.com/python-api-reference/generated/plotly.graph_objects.Bar.html) 旁邊以文字標出逾期和未完成。不要把日期型 `px.timeline` 當成數值模擬器或公平性證明。若要四種策略上下對齊，必須共用相同 x 軸範圍和事件資料；同步 zoom／cursor 與播放動畫屬加分功能，不是核心驗收項目。

## 最小畫面與停止線

最小畫面仍維持既定四區：

1. 左上：第 30 單位以前可見的佇列摘要、情境／快照識別碼。
2. 右上：AI 的兩個 0–2 權重、理由、可見資料範圍、呼叫狀態；逾時或錯誤明寫「不可評估」。
3. 下方：FIFO、Priority、開發情境由九組搜尋選定的一組權重、AI 四列同格式指標（逾期、未完成、最長等待、輔助 p95）及逐條接受／拒絕原因；九組完整結果另以可展開表或匯出資料呈現，不合併成第三列。
4. 側邊：兩節點 numeric lane；若時間不足，退化為事件表，不刪公平比較與拒絕結果。

不做：即時佇列接入、策略逐件動畫、可執行任意程式碼、模型改門檻、線上部署、多使用者、複雜 dashboard routing。動畫不應混入模擬等待時間；展示播放時間另計。

## 七小時內的成本估計與取消條件

估計以「開賽後模擬器和評測器已有穩定 JSON 輸出、固定工作表已封存、只一位 UI 實作者」為前提：Streamlit 先花 30–60 分鐘接四區和表格，再花 30–60 分鐘接一條 numeric lane、錯誤狀態和匯出；Gradio 相近，但精細排版較可能退化；React 方案要先付出 2–4 小時建立資料契約、啟動方式和前後端錯誤處理。這些是規劃假設，不是測量結果；「React skeleton」指開賽後其他工作交付的骨架，不是賽前可帶入的競賽實作。

停止／取消條件：

- 11:35 仍沒有可顯示的假資料：取消動畫、同步縮放和漂亮卡片，只做固定表格與文字事件紀錄。
- 13:35 真正成功執行並保存的結果尚未有穩定 JSON：UI 只能接「介面示意」假資料來排版，並明示它不是驗證證據，也不是 AI 輸出；不得稱為已記錄結果。只有成功執行後保存的資料才可稱「已記錄結果」。
- 14:35 前 Streamlit 尚未穩定而 React／ECharts 需要重新搭骨架：不換棧，退回 Streamlit 或純 HTML 表格。
- 15:35 核心比較尚未端到端重跑：凍結畫面在表格、收據和拒絕狀態，不再加入時間線功能。

## 仍需團隊決策與查證限制

團隊需決定：是否有人能承擔 Python 前提；是否接受本機啟動而非公開網址；時間線是否只要靜態 numeric lane；是否必須匯出一張圖片／JSON；模型不可用時要展示已記錄 AI 結果還是只展示失敗狀態。這些是開放問題，不能由本研究替團隊核定。

本研究查的是 2026-09-08 可公開讀到的官方文件；沒有安裝套件、沒有做效能或離線實測，也沒有驗證當天網路、瀏覽器、Node／Python 環境。React 靜態展示仍須預備本機 HTTP 服務；外部字型、CDN 或遠端 API 都可能令離線展示失敗。Gradio、Streamlit、React 圖表庫的版本在比賽前可能改變；正式實作時應鎖版本並做一次最小煙霧測試。工具文件證明「具備某功能」，不證明七小時內一定做完，也不證明任何策略效果。

本地對照文件：[候選產品規格](../product/task-scheduler-validation-candidate.md)、[團隊準備狀態](../operations/team-readiness.md)。

## 完成紀錄

- 修改檔案：`docs/research/2026-09-08-scheduler-ui-tools.md`（本檔唯一新增／修改檔）。
- 命令：`git diff --check -- docs/research/2026-09-08-scheduler-ui-tools.md`；結果：通過。
- 命令：`! rg -n '[[:blank:]]+$' docs/research/2026-09-08-scheduler-ui-tools.md`；結果：通過，沒有行尾空白。
- 未安裝套件、未實測、未提交。

白話比喻：Streamlit 像把既有廚房的食譜、計時器和出餐清單放到同一張工作桌，最容易在七小時內把「同一批訂單分成四種做法」講清楚；React 像重新裝一間漂亮的展示廚房，控制更細，但要先接好水電。若廚房停電（模型或網路失敗），我們仍應能拿出已印好的出餐清單和拒絕理由，而不是假裝廚師剛剛成功做完。
