# 排程輸送帶：實作交接 PRD

> 狀態：使用者於 2026-09-10 選定 `Hackathon_PRD.md` 為權威來源；本頁是忠實濃縮交接。來源 SHA-256：`d7466a4d01a8c359038ae552bad50ebe9f819751f26f94fb1a872a0ea14927ae`（capture：2026-09-10）。
>
> 本頁是產品實作規格，不是賽事規則或賽事驗收聲明。產品決策待主辦方回覆題目變更請求；主辦方回覆、團隊接受、凍結皆 pending。使用者回報咏宸的 UI 完成狀態尚未驗證。

## 目標與範圍

做單一 worker（一次只能處理一筆訂單的處理者）排程展示，讓人看懂訂單到達、等待、選擇規則與完成／逾期結果。三個必要 tab（分頁）：**Scheduling Arena**（現在）、**Metrics & Code**（本局結果與規則）、**Skill Library**（規則、來源、預覽與套用）。

交付：可在本機啟動的 Gradio 介面、固定依賴與命令；隊友 backend（後端服務）的相容 adapter（轉接層）、可明確選擇的 local simulator（本機模擬器）、共用契約測試；三個 tab、五個 policy（選下一筆訂單的規則）、播放控制、SVG、Mock（預寫示範）提案／接受／拒絕；pytest、Ruff 與瀏覽器驗收紀錄（標明 backend source）；README、`CONTEXT.md`、串接與操作文件、可重播的 90 秒 demo。隊友開工須提供同程序 Python backend；只完成 local 不算完成。

不做真實 LLM、完整 evaluator、動態 Python、Round Robin、搶占、worker failure、多 worker、登入、公開部署。Mock 接受只是套用預寫策略，不是改善證明；Hybrid 不保證較佳。

## 環境與啟動

Windows、Conda `scheduler-ui`、Python 3.11.16；uv 指向 Conda Python，不建 `.venv`。`requirements.txt` 固定 `gradio==6.26.0`、`plotly==7.0.0`、`pytest==9.1.1`、`ruff==0.16.6`（來源文件記載，未另作外部版本驗證）。

```powershell
conda create -n scheduler-ui python=3.11.16 pip -y
conda activate scheduler-ui
uv pip install --python "$env:CONDA_PREFIX\python.exe" -r requirements.txt
uv pip check --python "$env:CONDA_PREFIX\python.exe"
python app.py --backend team --factory team_backend:create_backend
python app.py --backend local
```

既有環境可略建立。不可寫死帳號／絕對路徑。`--backend` 必填；team 缺 factory 或載入失敗要明確錯誤，**絕不 fallback/local/mock**。預設 `127.0.0.1:7860`、無公開 share、`--port` 可改。每頁持續標「資料來源：隊友後端／本機備援」及「Planner／Evaluator：MOCK 示範」。

## 領域、事件與 policies

Job／Request 同物件；時間都是模擬秒。`arrival` 入等待列；`processing_time` 已知工時；priority 越大越高；`deadline` 絕對時間。單 worker 每次一筆，開始後不搶占。run 是初始化／reset 至下一次 reset。

僅有限數字；拒絕 NaN、Infinity、bool 偽數字、非正工時與 `deadline <= arrival`；ID 非空且同 run 唯一。建議十進位字串轉 Decimal，對外仍 JSON numbers；禁止顯示前四捨五入。狀態依序可為 `scheduled,pending,running,completed,expired`，後二終態。

worker 僅從已到達 pending 選 `now + processing_time <= deadline` 可行者；不可行者等到 deadline 才丟棄，無候選要顯示閒置原因。相同時刻**完成 → 丟棄 pending 逾期 → 納入到達 → 派工**；恰 deadline 完成成功。初始化及 reset 在 t=0 也到達與派工。`advance(dt)` 處理區間內所有事件；不同 update interval 不得改選擇、完成時刻、metrics。policy 切換不打斷 running，下一次派工才生效；無額外運輸時間。

| id | 顯示 | 排序（priority 外升冪，最後 ID 字典序） |
| --- | --- | --- |
| `fifo` | FIFO | arrival、ID |
| `sjf` | SJF | processing_time、arrival、ID |
| `priority` | Priority | priority 降冪、arrival、ID |
| `edf` | EDF | deadline、arrival、ID |
| `hybrid` | Hybrid | priority 降冪、deadline、processing_time、arrival、ID |

預設 seed 為 42、初始 8 筆 Job；每個 adapter 使用自己的 RNG（隨機數產生器），不可共享全域 RNG。初始資料可以與影片不同，但必須有效、可重現，且在有限模擬時間內結束。`generate(1)` 與 `generate(4)` 於目前 `now` 加入訂單；生成規則固定並記在串接文件；相同 seed、操作時刻與操作序列須可重現。總上限 20 筆，含尚未到達、running 與終態；終態不釋放名額。inject 前整批驗證；ID 衝突、資料無效、`arrival < now` 或超額時整批拒絕，不消耗 RNG、不可部分加入。reset 建新 run：time=0、8 Jobs、FIFO、四個 basic skills，清除 events、series、diff、uses 與 Mock 狀態；UI 暫停、Library 預覽 FIFO、倍速保留。重現比較排除 run ID 與 revision；不同 backend 無須使用相同隨機資料，但須用共用測資驗證規則。

## UI

Arena 是 Gradio Blocks＋單一 `gr.HTML` SVG 深藍舞台；JS 只在已知 snapshots 間動畫；清楚文字對比與灰／綠／黃，無須 pixel-match。1440×900 驗收；1280×720 控制可換行不重疊。主頁無橫向 overflow；訂單區約 420px 獨立捲動且更新保留位置。

Arena 依序有：共用頁首、資料來源與 MOCK 標籤、tab、播放／暫停／單步 +1／reset／0.5×、1×、2×、policy 選單、加 1 筆、Flash Sale 4 筆、Mock 區、舞台（policy、time、capacity、四張指標卡與 worker）、活動列與垃圾桶／EXIT、終態歷史。初始為暫停、1×；單步固定推進 1 模擬秒並停止連續播放，不乘倍速；按鈕樣式不能是播放狀態的唯一依據。

- 列序 running、pending、scheduled，再 arrival、ID，僅閱讀、非派工；未終態都顯示 ID、P、工時、deadline。
- scheduled：灰、起點、「抵達倒數 X 秒」；pending：綠、等待帶、「可準時完成／已無法準時完成」；running：黃、經 worker 至 EXIT；終態移出活動列，留 ID、`EXIT ✓`／「垃圾桶／回收」和終止時間。垃圾桶永久可見，標「截止 t=X」「截止後丟棄」。閒置區分未到達／全不可行／全局結束。
- 等待位置 `clamp((now-arrival)/(deadline-arrival),0,1)`；處理 `clamp((now-started_at)/processing_time,0,1)`；priority 不改位置，動畫不回寫完成。
- 只能有一個約 200 ms 的 timer；每次更新推進 `0.2 * speed` 模擬秒，畫面 `t=` 是權威。requestAnimationFrame（rAF）只能在前後 snapshot（同一時刻的完整狀態）之間插值，禁止預測未知未來。暫停時停止 timer 與插值；切換 tab 不新增 timer，也不自動暫停。全局完成不清局，仍可注入；繼續播放時模擬時間前進，throughput 可下降。

Metrics tab 的順序為：指標卡、三層圖（completed/expired、throughput、P95；共用模擬秒 X 軸）、code、diff。series 保存事件時刻的取樣（同時刻可合併）；UI 顯示最近 60 點和現在點，都是本局累計值，不是滑動視窗。所有卡、圖、policy 名稱和 code 必須來自同一份已接受的 snapshot。code 是實際 callable（可呼叫規則函式）的原始碼，或與版本綁定的靜態原始碼；須說明可行性篩選由 backend 共用 dispatch（派工）流程處理；不可執行展示文字。diff 是最近一次不同 policy 切換的 unified diff；未切換時顯示「尚無策略變更」。Mock 與真實 metrics 分開；中途切換不清除 metrics，不能當作新策略的獨立改善。

| metric | 定義 |
| --- | --- |
| completed/expired | 本局終態累計，整數件 |
| throughput | `completed/(now/60)` 件／模擬分鐘；now=0 為 None/`—`，now>0 無完成為 0 |
| P95 latency | completed 的 `completed_at-arrival`；空 None/`—`；排序 index `0.95*(n-1)` 線性插值；一筆即該值 |

卡片小數顯示兩位；測試比較原始值。Library 的順序是選單、規則／source／uses、code、套用；選單僅預覽，套用走與 Arena 相同的流程。uses 是實際派工次數，不是點擊次數，並顯示最近實際派工時刻。

Mock stages：`idle`（accept/reject disabled）、`proposed`（展示預寫 Hybrid 及「示範候選，未經真實 evaluator 驗證」；接受／拒絕可按）、`accepted`（登錄／套用，更新 code/diff/reason/Library）、`rejected`（策略／Library 不變，可再提）。propose 不切換、不捏造分數；只可在 proposed resolve，否則清楚錯誤。已登錄 Hybrid 重複接受不重複；拒絕新案不刪舊 Hybrid；reset 才回 4 skills。

## BackendAdapter

資料流為 `Gradio controls → session controller → BackendAdapter → team backend → one snapshot → three tabs`。session controller（每個瀏覽器工作階段的操作協調者）負責序列化操作；snapshot（某一時刻的完整可序列化狀態）是三個 tab 的共同資料來源。UI 不得自行派工或另算權威 metrics；adapter 轉換隊友物件、狀態與例外，不能用 local 結果冒充 team。

```python
create_backend(seed: int = 42, initial_jobs: list[dict] | None = None) -> BackendAdapter
```

每新瀏覽器 session 呼叫一次；`None` 建8、明確 list 只用該 list、`[]` 空。回傳前完成初始化和 t=0 事件。隊友不收 initial_jobs 時 adapter 須做等效測試初始化，不能跳過；不可回跨-session mutable singleton 或 deepcopy 鎖／外部資源。

公開 API 的回傳與語意如下；舊 backend 若成功時回傳 `None`，adapter 必須再取得並轉換 snapshot。

| 方法 | 回傳 | 必要語意 |
| --- | --- | --- |
| `reset(seed=42)` | `Snapshot` | 建新預設 run，不沿用測試 `initial_jobs`。 |
| `advance(dt)` | `Snapshot` | dt 為有限非負數；處理整個區間事件；0 不推進。 |
| `inject(jobs)` | `Snapshot` | 原子驗證並加入；當下到達的 Job 立即參與事件處理。 |
| `generate(count)` | `Snapshot` | count 僅內建整數 1 或 4。 |
| `set_policy(id, reason="手動切換")` | `Snapshot` | 驗證 skill 存在，記錄實際切換；同 policy 無變更；不搶占。 |
| `snapshot()` | `Snapshot` | 無副作用，回傳與內部狀態分離的可序列化資料。 |
| `list_skills()` | `list[Skill]` | 回傳本 run 可用 skills 與使用紀錄；讀取不改變狀態。 |
| `propose_candidate()` | `Snapshot` | 提出 Mock Hybrid 候選並記錄事件，不直接套用。 |
| `resolve_candidate(accept)` | `Snapshot` | accept 必為 bool，依 Mock 流程接受或拒絕。 |

公開 numbers 僅 built-in int/float，排 bool、數字字串、Decimal；內部 Decimal 轉換。snapshot 僅 dict/list/str/bool/int/finite float/None；拒絕 float 範圍外、轉換後工時非正、或 deadline/arrival 邊界失真；seed 非負 int 且排 bool。

| JobInput 欄位 | 型別與限制 | Job snapshot 補充欄位 |
| --- | --- | --- |
| `id` | 非空 `str`，同 run 唯一 |  |
| `arrival` | `number >= 0`；注入時不可早於 now |  |
| `processing_time` | 正的有限 `number` |  |
| `priority` | 有限 `number` |  |
| `deadline` | 有限 `number`，且大於 arrival |  |
| `status` | UI 不得輸入 | backend 決定的狀態字串 |
| `started_at` | UI 不得輸入 | 未派工為 null |
| `completed_at` | UI 不得輸入 | 僅 completed 有 number |
| `dropped_at` | UI 不得輸入 | 僅 expired 有 number |
| `feasible` | UI 不得輸入 | pending 用 `now + processing_time`；running 用 `started_at + processing_time`；scheduled／終態為 null |

JobInput 僅含前五欄，UI 只傳入這五欄。

Snapshot 欄位如下：

| 欄位 | 必要內容 |
| --- | --- |
| `run_id`、`time`、`jobs` | 新 run 的字串 ID、目前模擬秒、此局全部 Job。 |
| `worker` | `{job_id, state: "idle"|"running", reason}`。 |
| `policy` | `{id, name, code, previous_code, reason}`；無前一份 code 時 `previous_code=""`。 |
| `metrics` | `completed`、`expired` 為 int；`throughput`、`p95_latency` 為 number 或 null。 |
| `events`、`series` | 有序 events；series 按 time 排序，每點含 time 與四個 metrics。 |
| `adaptation` | `{stage, message, candidate_id}`。 |
| `capacity` | `{total, limit: 20, remaining}`，其中 remaining=limit-total。 |

Event 是遞增 `seq`、`time`、`type`、`job_id`、`policy_id`、`message`；type 只可為 `arrived`、`started`、`completed`、`expired`、`policy_changed`、`candidate_proposed`、`candidate_accepted`、`candidate_rejected`，無關 ID 使用 null。reset 使用新 run 和空 events 表。Skill 是 `id,name,description,code,source,uses,last_applied_at`；source 只能為 `base` 或 `mock`，且不等於 team/local 資料來源；uses 為非負整數，last_applied_at 是最近實際派工時刻或 null。

## Session／錯誤

每個 session controller 持有唯一 adapter，所有修改操作都要序列化；每次成功操作後，在同一序列化區間讀取 snapshot 與 skills，組成一次 UI update。UI envelope 另有 `session_generation`、`revision`、`backend_source`、`playing`、`speed`、`error`；revision 單調增加，reset 不歸零。reset 改變 generation 與 run、取消舊插值；前端只接收目前 generation、run_id、非過時 revision 的完整一致更新，SVG、metrics、code、選單皆同。

驗證失敗或已證實回滾的操作失敗要保持原狀。一般錯誤暫停並顯示「操作未完成，已暫停；保留最後有效畫面」，記錄 log，提供只讀的「重新取得狀態」與 reset；不可重送 inject。若 backend 未能證明失敗前後沒有修改，必保留最後有效畫面並明標「狀態待確認」；先以只讀方式重新同步，或 reset 後才接受新的修改，禁止盲目或自動重試原命令。初始失敗顯示連線錯誤空畫面，不造 snapshot；切換 team/local 必須重啟新 session，不混合結果。

失敗只拋例外、不可回偽成功 snapshot：`AdapterValidationError(ValueError)` 表示非法輸入／狀態且 jobs/RNG/events/time 不變；`AdapterOperationError(RuntimeError)` 有 `state_uncertain: bool`，僅證明未改才 False，預設 True。未分類例外與修改成功後讀取失敗皆 uncertain；controller 捕捉、記錄、暫停，失敗也新 revision；過時回應不更新。sync 後以實際 backend events 為準，絕不自動重送。

## 契約驗收、完成與 demo

local/team 都以 factory `initial_jobs` 跑同測資；缺 team 時 team 測試須失敗，不可 skip 後宣稱通過。

1. **基本策略。** A-D 都在 t=1 到達，`(processing_time, priority, deadline)` 分別為 A=(6,2,25)、B=(2,1,12)、C=(4,5,18)、D=(3,3,9)。在 t=0 選 policy、推進至 t=1，首筆必為 FIFO=A、SJF=B、Priority=C、EDF=D、Hybrid=C；Hybrid 測試須先提案並接受。
2. **同時刻事件。** A=(arrival=0, processing_time=2, priority=1, deadline=2)、B=(0,3,1,2)、C=(2,1,1,4)。t=0 派 A；t=2 A completed、B expired、C arrived 並 start；C t=3 completed。
3. **metrics。** FIFO，A=(0,2,1,10)、B=(0,4,1,10)：t=6 時 completed=2、expired=0、throughput=20、P95=5.8；t=12 時 throughput=10、P95 仍為 5.8。
4. **不搶占。** A 在 t=0 開始、processing_time=5、deadline=20；B／C 在 t=1 到達、processing_time 分別為4／1、deadline 都為20。在 t=1 改 SJF，A 仍 t=5 完成，下一筆選 C。

pytest 至少覆蓋四例、全部 tie-break 與 Hybrid 層級、不可行等待、deadline 邊界與 idle、0.3 工時以 0.1 分段和不同 advance 步長的等價性、P95／throughput 的 null 與零值、seed 重現與失敗注入不耗 RNG、20 筆上限、所有非法值、Mock、雙 factory 隔離、防禦性副本與序列化、controller controls/generation/revision、公開數字限制與有限 JSON、uncertain 復原，以及 team 真呼叫、無 fallback、驗收輸出中的資料來源。

瀏覽器驗收包含：team 模式三個 tab、資料來源與 Mock 標籤；狀態動畫；全部 controls 與 capacity；20 筆資料與 scroll；切 tab 不重複推進；strategy、code、diff、Library、metrics 同步；reset 後舊回應不回灌；暫停可讀；結束後續播造成 throughput 下降；故障 adapter 的文案、保留畫面與 sync；真 team 模式沒有降級。留下 team 模式 90 秒流程、測試輸出、操作紀錄與必要截圖。只有所有功能／驗收、真 team 證據和可重啟文件都具備，才可稱完成；未通過必列出。

```powershell
conda run -n scheduler-ui python -m pytest -q --backend local
conda run -n scheduler-ui python -m pytest -q --backend team --factory team_backend:create_backend
conda run -n scheduler-ui python -m ruff check .
conda run -n scheduler-ui python -m ruff format --check .
```

安排：0–1 小時環境、factory、最小動畫；1–2.5 小時契約、adapter、對照、核心測試；2.5–4.5 小時 Arena/controller；4.5–6 小時 Metrics/Library/Mock；6–8 小時驗收、復原、文件、演練。開工確認 module、factory、依賴與可呼叫例；未提供即記錄 blocker，可做 local 開發但不改變 team 必接通。落後先減少裝飾，不能刪 tabs、動畫、策略、Mock 或驗收。

90 秒 demo：0–15 顯示 team／Mock、reset/play；15–30 Flash Sale 4，說明 deadline；30–45 propose Mock Hybrid；45–60 accept，觀察下一次 dispatch（不打斷現有工作）；60–75 暫停後開 Metrics，展示實際 code、diff 與本局 metrics（不宣稱改善）；75–90 開 Library，說明預覽與套用。固定注入時刻與操作序列以確保可重現，無須重現影片數值。交付說明須列安裝／啟動、實際 factory、生成規則、已跑驗收與結果、阻礙，並明確區分 local demo 和未完成的 team 串接。
