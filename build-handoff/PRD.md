# 排程輸送帶：唯一實作交接 PRD

> 權威文件：本檔為未來 implementation repo 唯一 PRD。2026-09-10 依使用者決定，由 `Hackathon_PRD.md` 遷入；遷移前來源可由 Git 歷史（基線 `088ae4cdf7c9f6cbb439a70630bfff60c49d09d9`）取得，非現行規格版本。
>
> 狀態：本次條款已獲使用者授權定稿；UI 完成僅為使用者回報，尚未驗證。這不等於全隊共同確認、規格凍結或整體完成。

## 1. 產品目標與交付邊界

### 1.1 要解決的問題

讓評審與隊友看懂：訂單何時到達、為什麼等待、worker 如何按不同策略挑選訂單，以及完成或錯過期限如何影響結果。

可以將系統想成一位店員處理多張有期限的訂單。上方等待帶表示時間壓力，下方處理路徑表示店員正在工作；店員每次只服務一筆訂單。

三個畫面分別回答：

- **Scheduling Arena**：現在發生什麼事？
- **Metrics & Code**：整局結果如何？目前按什麼規則派工？
- **Skill Library**：有哪些規則？如何查看與套用？

### 1.2 必須交付

- 本機可啟動的 Gradio 應用程式、依賴版本與啟動命令。
- 隊友後端的相容 adapter、可明確選擇的本機模擬器，以及共用的契約測試。
- 三個 tabs、五種策略、播放控制、SVG 動畫與 Mock 提案／接受／拒絕流程。
- pytest、Ruff 與瀏覽器驗收紀錄；紀錄必須指出使用的後端來源。
- README、`CONTEXT.md` 領域詞彙、後端串接文件與 UI 操作教學。
- 一段可重複操作的 90 秒展示流程。

**隊友於開工時提供可呼叫的同程序 Python 後端。只完成本機模擬器，不算完成整體交付。**

本包涵蓋真實排程、指標、隊友後端串接及 Mock 流程；不可憑 Mock 宣稱整隊已完成「自主且具適應能力的 AI」。真正適應所需的觀察、動作、評估證據與 owner，仍由全隊界定；本 PRD 不自行新增 learning、GPU 或完整 evaluator 需求。

### 1.3 不納入本版

不接真實 LLM、不建立完整 evaluator、不執行動態產生的 Python，不加入 Round Robin、搶占、worker failure、多 worker、登入系統或公開部署。

Mock 候選可以被接受，但這只表示使用者選擇套用預寫策略，不代表系統已證明改善。Hybrid 不保證優於其他策略。

## 2. 環境與從零啟動方式

完整環境與啟動方式移至 [實作交接包 README](README.md#環境與從零啟動)。該 README 為本節唯一作業位置。

## 3. 領域模型與排程規則

### 3.1 基本定義

| 詞彙 | 定義 |
| --- | --- |
| Request／Job | 一筆可以被派工的訂單；兩個名稱指同一個領域物件。 |
| Arrival | 訂單進入可等待佇列的模擬時間。 |
| Processing time | worker 完成訂單所需的已知工時。 |
| Priority | 數值越大代表優先度越高。 |
| Deadline | 訂單必須完成的模擬時間，為絕對時間而非倒數秒數。 |
| Worker | 唯一處理者，每次只處理一筆，開始後不中斷。 |
| Policy／Skill | 決定下一筆訂單的排程規則；技能庫中的 skill 對應一個可用 policy。 |
| Run | 從初始化或重設到下一次重設之間的一局。 |

所有時間單位為模擬秒，與影片秒數或電腦時鐘區分。數值必須有限；拒絕 NaN、Infinity、布林值冒充數字、非正工時，以及 `deadline <= arrival`。ID 必須非空且在同局唯一。

### 3.2 生命週期與派工

狀態為 `scheduled`（尚未到達）、`pending`（等待）、`running`（處理）、`completed`（成功）、`expired`（逾期丟棄）。後兩者為終態。

worker 空閒時，只從已到達的 pending 訂單中，先篩選：

```text
now + processing_time <= deadline
```

再按以下排序選出第一筆：

| Policy ID | 顯示名稱 | 由前到後比較，除 priority 外均為由小到大 |
| --- | --- | --- |
| `fifo` | FIFO | arrival、ID |
| `sjf` | SJF | processing_time、arrival、ID |
| `priority` | Priority | priority 由大到小、arrival、ID |
| `edf` | EDF | deadline、arrival、ID |
| `hybrid` | Hybrid | priority 由大到小、deadline、processing_time、arrival、ID |

ID 平手排序使用字串字典序。不可行訂單繼續等待，直到 deadline 才丟棄；沒有合格訂單時 worker 閒置並顯示原因。

同時刻事件順序固定為：**完成 → 丟棄逾期等待單 → 納入新到達單 → 派工**。恰好在 deadline 完成算成功。初始化與 reset 在 `t=0` 也處理到達及派工，所以暫停畫面可以已有 running 訂單，但工時尚未推進。

切換 policy 不搶占目前工作，於下一次派工生效。輸入與工時精確已知，不增加額外運輸耗時。

`advance(dt)` 必須處理整個區間內的所有事件，不能只在區間末端判斷。不同更新間隔不得改變選單結果、完成時間與指標。建議內部採用由十進位字串建立的 Decimal，避免累加小數造成 deadline 邊界錯誤；對外維持可 JSON 序列化的數字。

### 3.3 場景、名額與重設

- 預設 seed 為 42，初始八筆訂單。每個 adapter 有自己的隨機產生器，不共享全域 RNG。
- 起始資料的生成規則可由各後端實作，但必須有效、可重現，且能在有限模擬時間內結束。不要求與原影片相同。
- `generate(1)`、`generate(4)` 在目前時間加入訂單；產生規則須固定並記錄於串接文件。相同 seed、相同操作時間及操作序列應可重現相同結果。
- 累計上限二十筆，包含尚未到達、處理中與已結束訂單。完成／回收不釋出名額。
- 注入前先驗證整批。ID 衝突、資料無效、到達時間早於現在或超過名額時，整批拒絕，不消耗 RNG、不留下部分新增。
- reset 建立新 run，清除舊 run 的 jobs、RNG、events、指標歷史、策略差異、使用紀錄與 Mock 狀態，再以傳入 seed（預設 42）建立八筆、FIFO 與四個基本 skills，並在 t=0 處理該局全部事件；因此新 run 可已有 running 工作、新事件與 uses。新 run 的 event seq 自 1 起算。UI 暫停並將技能預覽回到 FIFO；播放倍速保留目前選擇。session revision 單調遞增、不因 reset 歸零；測試用 initial_jobs 不沿用。
- 重現性比較排除 run ID、更新版本等識別資訊；不要求不同後端使用相同隨機資料。規則一致性用共用的明確測試資料驗證。

## 4. UI 與互動需求

### 4.1 Scheduling Arena

使用 Gradio Blocks 控制項與單一 `gr.HTML` 動畫舞台。舞台內使用 SVG 表示輸送帶，JavaScript 只補已知快照間的動畫。保留深藍色舞台、清楚的文字對比、灰／綠／黃狀態色，不要求像素完全一致。

桌面參考版面由上到下如下，其他 tabs 共用頁首與來源標籤：

```text
標題、說明、資料來源與 MOCK 標籤
Arena | Metrics & Code | Skill Library
播放 | 暫停 | 單步 +1 | 重設 | 倍速
排程策略下拉選單 | 新增 1 筆 | Flash Sale 4 筆
Mock 狀態、候選說明、提出／接受／拒絕
深色舞台：策略／時間／名額 → 四張指標卡 → worker 狀態
可捲動訂單列：左側 ID／P／工時；上方等待帶 → 垃圾桶
                                      下方處理路徑 → EXIT
本局完成／回收歷史
```

以 1440×900、瀏覽器 100% 縮放驗收；另在 1280×720 確認控制項可換行且不重疊。主頁可垂直捲動，訂單列區約 420 px 高並獨立捲動；不得出現整頁水平溢出或文字蓋住垃圾桶／EXIT。Metrics tab 依序為指標卡、三層趨勢圖、code、diff；Library 依序為選單、規則及來源紀錄、code、套用按鈕。

上方控制區提供播放、暫停、單步 +1、重設、0.5×／1×／2×、排程策略下拉選單、新增一筆、Flash Sale 四筆。初始為暫停、1×。單步永遠推進一模擬秒，不乘倍速，並暫停連續播放以便觀察。

舞台顯示「下次派工策略」、模擬時間、播放狀態、最近切換原因、本局累計名額、四張指標卡與 worker 狀態。切換立即影響下一次派工，不中斷 running。按鈕本身是否為主色不能作為播放狀態的唯一依據。

每筆未結束訂單一列，列上顯示 ID、P 優先度、工時與 deadline。排列順序為 running、pending、scheduled，再依 arrival、ID；此順序只為閱讀方便，不代表派工順序。超出舞台高度時內部捲動，不抽樣或隱藏訂單；更新時保留捲動位置。

| 狀態／元素 | 視覺及文案 |
| --- | --- |
| scheduled | 灰色方塊停在起點，顯示「抵達倒數 X 秒」。 |
| pending | 綠色方塊沿上方等待帶移動，顯示「可準時完成」或「已無法準時完成」。 |
| running | 黃色方塊離開等待帶，經 worker／齒輪標記沿下方處理路徑前往 EXIT。 |
| 等待帶終點 | 永久可見的垃圾桶 SVG 圖示，旁邊標示「截止 t=X」「截止後丟棄」。 |
| completed／expired | 移出活動列，保留 ID、`EXIT ✓` 或「垃圾桶／回收」以及結束時間。 |
| worker 閒置 | 顯示「目前沒有可派工訂單」並區分尚未到達、全部不可行或本局工作皆結束。 |

等待位置為 `clamp((now-arrival)/(deadline-arrival), 0, 1)`；處理進度為 `clamp((now-started_at)/processing_time, 0, 1)`。不得用 priority 改變等待位置，也不得將動畫自身的到站判斷回寫為完成事件。

使用約 200 ms 的單一更新來源；每次播放更新推進 `0.2 × speed` 模擬秒。實際觀看速度可能受機器負載影響，畫面 `t=` 才是權威時間。瀏覽器可用 requestAnimationFrame 在前後快照間短暫插值，禁止向未知未來外推。暫停停止連續推進與插值；切換 tab 不新增 timer，也不自動暫停。

已結束所有訂單時不自動清局；仍可注入剩餘名額。若繼續播放，模擬時間繼續走，吞吐量可能下降。

### 4.2 Metrics & Code

所有卡片、圖表、策略名稱與 code 必須對應同一份已接受快照。

| 指標 | 計算與顯示 |
| --- | --- |
| 完成數 | 本局 completed 的累計數，單位件。 |
| 逾期數 | 本局 expired 的累計數，單位件。 |
| 吞吐量 | `completed / (now / 60)`，單位件／模擬分鐘；`now=0` 為 None／「—」，`now>0` 且無完成時為零。 |
| P95 完成延遲 | 只取 completed 的 `completed_at-arrival`，單位秒；空集合為 None／「—」。 |

P95 使用線性插值：將 n 筆延遲排序，取索引 `0.95*(n-1)`，在相鄰值間插值；只有一筆時就是該值。卡片小數顯示至兩位，數量使用整數；測試比較原始值而非格式化文字。

趨勢圖由上到下為完成／逾期數、吞吐量、P95，共用模擬秒的 X 軸，卡片與圖表都標示「本局累計」。後端在領域事件後保存取樣；同一模擬時刻只保留該時刻全部事件／操作後的最終狀態，後續操作覆寫該點。無事件的 advance 不新增永久取樣，但目前點仍更新吞吐量。UI 只使用後端 `snapshot.series`，再以 `snapshot.time`／`snapshot.metrics` 組目前點；按 time 以後者覆蓋去重後顯示最後六十點（含目前點），不得重算權威 metrics。每個點的指標仍是整局累計，不是滑動視窗平均。

唯讀 code 顯示實際執行的排序函式；明確說明可行性篩選由後端共用派工流程負責。由實際 callable 取得原始碼或提供與該版本綁定的靜態原始碼，不執行展示文字。策略差異顯示最近一次不同 policy 切換的 unified diff；尚未切換時顯示「尚無策略變更」。

Mock 訊息與真實指標分開。中途切換不清空指標，不能把整局結果當成新策略的獨立改善證據，也不得顯示無依據的改善百分比。公平策略比較必須用相同明確資料各自從頭跑完，作為測試／證據，不新增比較頁。

### 4.3 Skill Library 與 Mock adaptation

技能下拉選單只改變預覽；按「套用技能」才走與 Arena 相同的 `set_policy` 流程。顯示名稱、規則、code、來源、使用次數與最近使用的模擬時間。使用次數定義為該策略實際派工的次數，不是點擊套用的次數。

Arena 提供 Mock 區塊，狀態依序可為：

- `idle`：尚未提出候選，可提出；接受／拒絕不可按。
- `proposed`：展示預寫 Hybrid、規則與「示範候選，未經真實 evaluator 驗證」；接受／拒絕可按。
- `accepted`：登錄 Hybrid 並走共用切換；實際切至不同 policy 時更新 code、差異與切換原因，並更新技能庫；可再次提出候選。
- `rejected`：維持操作當下策略與技能庫內容；可再次提出候選。

Mock 只有單一預寫 Hybrid 候選。idle、accepted、rejected 可提出；proposed 再提出必須拋出 `AdapterValidationError` 且完全不修改，UI 禁用提出。只有 proposed 可接受／拒絕；直接重複 resolve 為明確錯誤。Hybrid 接受後加入 Arena 與 Library 兩處選單；accepted／rejected 可再次提出同一候選，重複登錄不新增 skill。手動切換不取消待決候選；提出與拒絕不切換策略、不編造評估分數；拒絕新提案不刪除先前已接受的 Hybrid。若接受時已是 Hybrid，仍記 `candidate_accepted`，但不新增 `policy_changed` 或 diff；reset 才恢復四個基本 skills。

## 5. 同程序 Python 後端契約

### 5.1 責任與 factory

```text
Gradio 控制項 → session controller → BackendAdapter → 隊友後端
                            ↓
                    一致快照 → 三個 tabs
```

UI 不得自行重新派工或重算一套權威 metrics。相容層負責將隊友物件、狀態名稱與例外轉為以下契約；不得以本機模擬器的結果冒充隊友結果。

每次新瀏覽器 session 都呼叫一次 factory：

```python
create_backend(
    seed: int = 42,
    initial_jobs: list[dict] | None = None,
) -> BackendAdapter
```

`initial_jobs=None` 建立八筆初始訂單；明確傳入清單時只使用該清單，`[]` 建立空場景。這個測試入口只供初始化／共同驗收；`reset()` 一律回到預設八筆，不沿用它。factory 完成初始化及 `t=0` 事件處理後回傳 adapter。

隊友若不原生接受 initial_jobs，由相容層提供等效的測試場景初始化；不能跳過規則測試。factory 不回傳跨 session 共用的可變 singleton。UI 不以 deepcopy 複製含鎖或外部資源的隊友實例。

### 5.2 公開方法

下列為新 adapter 的統一回傳約定；現有後端若回傳 None，由相容層在成功後取得 snapshot 轉換。

公開 Python API 的 number 僅接受內建 int 或 float，明確排除 bool、數字字串與 Decimal；隊友的 Decimal 等內部型別由相容層轉換。對外快照只包含 dict、list、str、bool、int、有限 float 與 None，禁止 NaN／Infinity。不得在回傳前為了顯示而四捨五入時間或 metrics；超過有限 float 可表示範圍、轉換後正工時變成零或 deadline／arrival 邊界失真的資料應拒絕。seed 為非負 int，排除 bool。

| 方法 | 回傳 | 必要語意 |
| --- | --- | --- |
| `reset(seed=42)` | Snapshot | 新 run，恢復預設場景；不沿用測試用 initial_jobs。 |
| `advance(dt)` | Snapshot | dt 為有限非負數，處理區間內全部事件；零不推進時間。 |
| `inject(jobs)` | Snapshot | 接收 JobInput 清單，驗證後原子加入；當下到達者立即參與事件處理。 |
| `generate(count)` | Snapshot | count 僅為整數 1 或 4，在 now 產生並注入。 |
| `set_policy(id, reason="手動切換")` | Snapshot | Arena 與 Library 共用；驗證 skill 存在，立即更新下次派工策略、不中斷 running。相同 policy 為 no-op，仍保留 reason 與既有 diff。 |
| `snapshot()` | Snapshot | 無副作用，回傳與內部狀態分離的可序列化快照。 |
| `list_skills()` | list[Skill] | 回傳同 run 可用 skills 與使用紀錄，讀取不改變狀態。 |
| `propose_candidate()` | Snapshot | 僅在 idle／accepted／rejected 提出單一 Mock Hybrid，記錄事件，不直接套用。 |
| `resolve_candidate(accept)` | Snapshot | accept 必須為 bool；僅 proposed 可接受或拒絕。 |

### 5.3 JobInput 與 Job

| 欄位 | 型別／值 | 語意 |
| --- | --- | --- |
| `id` | str | 同 run 唯一、非空 ID。 |
| `arrival` | number | 非負模擬秒；注入時不可早於 now。 |
| `processing_time` | number | 正的已知工時。 |
| `priority` | number | 有限數值，越大越優先。 |
| `deadline` | number | 模擬秒，必須大於 arrival。 |
| `status` | 狀態字串 | 僅 Job 快照包含，由後端決定。 |
| `started_at` | number 或 null | 尚未派工為 null。 |
| `completed_at` | number 或 null | 只有 completed 有值。 |
| `dropped_at` | number 或 null | 只有 expired 有值。 |
| `feasible` | bool 或 null | pending 用 now＋工時判斷；running 用開始時間＋工時判斷；scheduled 與終態為 null。 |

JobInput 僅含前五欄；UI 不傳入狀態或完成時間。

### 5.4 Snapshot

| 欄位 | 型別／必要內容 |
| --- | --- |
| `run_id` | str，每次 reset 改變，不作排程 tie-break。 |
| `time` | number，目前模擬秒。 |
| `jobs` | list[Job]，包含本局全部工作。 |
| `worker` | job_id 為 str 或 null；state 為 `idle` 或 `running`；reason 為 str。 |
| `policy` | `{id, name, code, previous_code, reason}`；無前一份 code 時使用空字串。 |
| `metrics` | completed、expired 為 int；throughput、p95_latency 為 number 或 null。 |
| `events` | list[Event]，本局有序事件。 |
| `series` | list，欄位為 time 與四個 metrics；後端事件取樣，依模擬時間排序，同時刻僅保留最終狀態。 |
| `adaptation` | stage、message 為 str；candidate_id 為 str 或 null；stage 使用第 4.3 節定義。 |
| `capacity` | `{total: int, limit: 20, remaining: int}`，remaining = limit − total。 |

以下是「明確注入一筆測試資料」於時間零的完整快照範例，不是八筆初始場景：

```json
{
  "run_id": "run-example",
  "time": 0,
  "jobs": [
    {
      "id": "A", "arrival": 0, "processing_time": 2,
      "priority": 3, "deadline": 3, "status": "running",
      "started_at": 0, "completed_at": null, "dropped_at": null,
      "feasible": true
    }
  ],
  "worker": {"job_id": "A", "state": "running", "reason": "FIFO 選中可準時完成的訂單"},
  "policy": {
    "id": "fifo", "name": "FIFO",
    "code": "def fifo_key(job):\n    return (job.arrival, job.id)\n",
    "previous_code": "", "reason": "初始策略"
  },
  "metrics": {"completed": 0, "expired": 0, "throughput": null, "p95_latency": null},
  "events": [
    {"seq": 1, "time": 0, "type": "arrived", "job_id": "A", "policy_id": null, "message": "訂單到達"},
    {"seq": 2, "time": 0, "type": "started", "job_id": "A", "policy_id": "fifo", "message": "開始處理"}
  ],
  "series": [{"time": 0, "completed": 0, "expired": 0, "throughput": null, "p95_latency": null}],
  "adaptation": {"stage": "idle", "message": "尚未提出 Mock 候選", "candidate_id": null},
  "capacity": {"total": 1, "limit": 20, "remaining": 19}
}
```

### 5.5 Skill 與 Event

Skill 欄位為 `id`、`name`、`description`、`code`、`source`、`uses`、`last_applied_at`。source 使用 `base` 或 `mock`，與全頁的 team／local 資料來源是不同概念。uses 為非負整數；last_applied_at 記錄最近實際派工時間，未使用為 null。

以下是與上方快照對應的單一 Skill 範例；`list_skills()` 還須包含其他可用策略：

```json
{
  "id": "fifo", "name": "FIFO", "description": "先到先服務",
  "code": "def fifo_key(job):\n    return (job.arrival, job.id)\n",
  "source": "base", "uses": 1, "last_applied_at": 0
}
```

Event 欄位為 `seq`（同 run 自 1 起的遞增整數）、`time`（模擬秒）、`type`、`job_id`、`policy_id`、`message`。沒有相關 ID 時用 null。

type 固定使用 `arrived`、`started`、`completed`、`expired`、`policy_changed`、`candidate_proposed`、`candidate_accepted`、`candidate_rejected`。reset 以新 run 與清空事件表表示，不將舊 run 事件帶入。

```json
{
  "seq": 7, "time": 4, "type": "policy_changed",
  "job_id": null, "policy_id": "edf", "message": "技能庫套用"
}
```

### 5.6 Session、舊回應與失敗處理

- 每個 session 的 controller 持有唯一 adapter，所有更新與控制共用序列化入口；一次只執行一個修改操作。`state_uncertain=True` 時，入口在執行每一筆操作前檢查鎖定，連已排隊但未執行的修改也不得送出；唯讀重新同步與 `reset` 是復原例外。
- controller 在每次成功操作後讀取 snapshot 與 skills，組成一份 UI 更新。讀取必須落在同一個序列化區間，不能讓三個 tabs 各自推進或抓到不同狀態。
- UI envelope 加上 `session_generation`、`revision`、`backend_source`、`playing`、`speed`、`error`；這些是呈現控制資訊，不取代後端 run_id。
- revision 在同一 session 內單調遞增，reset 不歸零。成功 reset 更新 generation 及 run_id，取消舊插值；排入舊 generation 的 timer／操作結果不得套用到新 run。若 reset 已成功但後續讀取失敗，重新同步發現新 run_id 時也更新 generation、取消舊插值與排隊更新，讓新 run 的完整一致快照可發布。
- 前端只接受目前 generation、run_id 與非過時 revision 的一致更新；防護涵蓋舞台、metrics、code 與選單，不能只擋 SVG。
- 驗證失敗必須完全不改 jobs、RNG、events、time 或其他狀態；後端修改失敗是否未改則只以後端證據判定。UI 不假設對隊友實例 deepcopy 就能回滾外部效果。
- 一般錯誤顯示「操作未完成，已暫停；保留最後有效畫面」。保留錯誤 log，提供「重新取得狀態」及「重設」。重新取得狀態只呼叫讀取方法，避免重送注入造成重複訂單。
- 若操作或讀取失敗而後端無法保證未修改，標示 `state_uncertain=True`，保留畫面並鎖定所有修改、播放、單步、注入、策略與 Mock；只允許唯讀重新同步或 reset。重新同步在同一序列化區間取得並驗證完整 snapshot 與 skills 後才解除鎖，仍維持暫停且絕不自動重送；完整 reset 成功同樣解除鎖並維持暫停。同步失敗繼續鎖定。若第一次初始化就失敗，顯示連線錯誤空畫面，不捏造初始快照。
- 更換 team／local 來源需明確重新啟動對應模式並開始新 session，不承接或混合上一個來源的 metrics。

### 5.7 例外與重試契約

成功回傳前述 Snapshot／Skill；失敗以 Python 例外回報，不回傳外觀像成功的錯誤快照。相容層統一為兩種例外，均附可讀訊息：

| 例外 | 語意與 UI 處理 |
| --- | --- |
| `AdapterValidationError(ValueError)` | 輸入或操作狀態不合法；jobs、RNG、events 與後端時間完全不變。顯示原因，使用者修正後可再操作。 |
| `AdapterOperationError(RuntimeError)` | 後端執行／讀取失敗；包含 `state_uncertain: bool`。只有後端能證明未修改時才設 False：保持暫停，使用者排除原因後可手動操作。True 為預設，須完整同步或 reset 讀回成功後才接受修改。 |

未分類的後端例外與「修改成功、後續讀取失敗」均視為狀態不明。controller 捕捉、記錄並暫停，不將堆疊內容當成使用者說明。失敗更新也取得新的 UI revision，使錯誤畫面能覆蓋舊播放畫面；被丟棄的過時回應不再更新 UI。

驗證失敗或可證明已回滾的操作不新增領域事件。狀態不明時，不假設後端事件沒有改變；重新同步後以後端實際事件為準，且不得自動重送上一筆修改。

## 6. 驗收案例與證據

驗收單元、子 PRD 相依與共同確認方式見 [ACCEPTANCE.md](ACCEPTANCE.md)，證據記於 [checklist.md](checklist.md)。本 PRD 全文定義必要行為，第 6 節是驗證方式而非唯一行為要求；案例不是逐項批准或勾選單位。

### 6.1 可手算的共同測試資料

以下案例在 local 與 team adapter 上都必須通過。用 factory 的 initial_jobs 建立獨立場景，不依賴隨機生成的八筆訂單。

**案例 A：基本策略。** 四筆都在 t=1 到達，在 t=0 選好策略後推進到 t=1：

| ID | arrival | processing_time | priority | deadline |
| --- | --- | --- | --- | --- |
| A | 1 | 6 | 2 | 25 |
| B | 1 | 2 | 1 | 12 |
| C | 1 | 4 | 5 | 18 |
| D | 1 | 3 | 3 | 9 |

預期首筆：FIFO=A、SJF=B、Priority=C、EDF=D、Hybrid=C。測 Hybrid 時先在 t=0 提案並接受。

**案例 B：deadline 與同時刻順序。** A=(arrival 0, 工時 2, deadline 2)、B=(0, 3, 2)、C=(2, 1, 4)，priority 都為 1。t=0 派 A；t=2 依序完成 A、丟棄 B、到達 C、派 C。B 從未開始；C 在 t=3 完成。

**案例 C：metrics。** FIFO；A=(0, 2, 10)、B=(0, 4, 10)，priority 都為 1。A 在 t=2 完成，B 在 t=6 完成；完成延遲為 [2, 6]。t=6 時完成 2、逾期 0、吞吐量 20 件／分鐘、P95=5.8 秒。繼續推進到 t=12 而不新增，吞吐量為 10，P95 仍為 5.8。

**案例 D：不搶占。** A 在 t=0 開始、工時 5、deadline 20；B、C 在 t=1 到達，工時分別 4、1，deadline 都為 20。在 t=1 切到 SJF，A 仍於 t=5 完成，下一筆選 C。

### 6.2 pytest 必要覆蓋

1. 案例 A–D；每種策略的 arrival／ID 平手，Hybrid 的 priority、deadline、工時逐級比較。

2. 不可行訂單保留到 deadline；沒有合格工作時閒置；恰好 deadline 完成成功。

3. 大步 advance 與整數／小數分段 advance 的最終 jobs、事件順序與 metrics 相同；包含工時 0.3、以 0.1 分段的邊界。

4. 零時間吞吐量為 None；正時間無完成為零；無完成 P95 為 None；單筆與多筆 P95 正確。

5. 相同 seed 與操作序列可重現；驗證失敗不改 RNG、jobs、events 或 time。

6. 20 筆上限；剩餘三名額不能加入四筆；終態不釋出名額。

7. 無效數值、布林值、零／負工時、重複 ID、過去 arrival、未知 policy、無效 count、負 dt 均明確拒絕。

8. Mock 為單一預寫 Hybrid：idle／accepted／rejected 可提出，proposed 再提出或非 proposed resolve 明確拒絕且不改狀態；接受登錄並共用切換，拒絕保持操作當下策略；重複登錄不新增 skill，已是 Hybrid 時不新增 policy_changed／diff；reset 移除 Hybrid 並清理歷史。

9. Arena 與 Library 共用 `set_policy`；僅預覽不切換；切換立即影響下次派工、正在執行工作不中斷；同 policy no-op 保留 reason／diff。

10. 兩個 factory 實例互不影響；snapshot 為防禦性副本；序列化成功。

11. controller 暫停、倍速、單步、reset、舊 generation／revision 拒收與例外恢復；reset 後讀取失敗時，重新同步的新 run_id 可安全更新 generation。

12. API 拒絕數字字串、Decimal 與 bool；輸出為有限 JSON 數值。驗證例外完全不改後端與 RNG；狀態不明時連排隊修改都阻止執行，成功取得完整 snapshot 與 skills 後才解除鎖並維持暫停。

13. team adapter 真的呼叫隊友後端；team 載入失敗不 fallback；驗收輸出包含後端來源。

復刻版測試入口須接受 `--backend`、`--factory`，由共用 fixture 選擇 adapter，同一份契約案例分別執行。缺少隊友後端時 team 驗收應失敗，不得 skip 後宣稱整體通過。

實作 repo 的 `uv` 命令見 [README「環境與從零啟動」](README.md#環境與從零啟動)；僅在相應入口存在時執行。

### 6.3 瀏覽器與完成定義

1. team 模式下三個 tabs 可使用，資料來源與 Mock 標籤始終可見。

2. 灰色等待到達、綠色等待 deadline、黃色處理到 EXIT、回收與歷史紀錄符合後端狀態。

3. 播放、暫停、單步、三段倍速、重設及注入名額正常；切 tab 不重複推進。

4. 二十筆資料全部可查看，畫面更新保留捲動位置；基本桌面視窗可讀，不要求手機像素對齊。

5. 策略、code、diff、Library、指標卡與圖表同步；圖表只用後端 metrics／series、按 time 去重取最後 60 點；reset 後舊回應不回灌，讀取失敗後同步到新 run 亦可安全發布。

6. 可在暫停時閱讀數值；全局結束後繼續播放造成吞吐量下降屬預期。

7. 以可控制的失敗 adapter 驗證錯誤文案、保留畫面、state_uncertain 鎖定及重新同步；另確認真實 team 模式沒有默默降級。

8. 90 秒流程在 team 模式走通，並留下測試輸出與操作紀錄；附件、截圖或影音由團隊依需要決定，不是必要條件。

**完成定義：所有必要功能與驗收通過、真實 team 串接已證實、文件足以讓隊友重啟。未通過的項目必須列出，不以畫面看起來可動代替契約驗證。**
