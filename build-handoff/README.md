# 實作交接包

此資料夾是規劃 repo 與未來比賽實作 repo 的唯一文件介面。產品規則以 [PRD.md](PRD.md) 為準；[ACCEPTANCE.md](ACCEPTANCE.md) 保留完整驗收案例；[checklist.md](checklist.md) 管整體與非功能關卡。

開賽後將本包的 `AGENTS.md`、`CONTEXT.md`、`ARCHITECTURE.md`、`WORKFLOW.md`、`README.md`、`PRD.md`、`ACCEPTANCE.md`、`checklist.md` 一起帶入新的實作 repo 根目錄，保留相對路徑。本包不依賴規劃 repo 的操作手冊、研究或個人檔案；後續以實作 repo 的文件與 GitHub 為準。

使用者已授權把原始 PRD 正式遷入本交接包。規格版本應記錄驗證／凍結實際使用的 canonical Git commit；目前待提交後填入。`088ae4cdf7c9f6cbb439a70630bfff60c49d09d9` 只可作原根目錄完整 PRD 的歷史來源，不是本次遷移後的 canonical 版本。不要再將根目錄檔案、複製雜湊或 capture 日期當成權威。根目錄只作導向入口。

本包描述 Gradio 排程展示：三個 tabs、五種固定策略、真實排程與指標、隊友後端串接，以及 Mock（預寫示範，非真實 AI）候選流程。隊友後端是完整交付必要條件；`local` simulator 僅可在明確選擇時作開發或備援展示。`team` 後端載入失敗必須明確失敗，不得靜默改為本機資料。Mock 不足以證明整隊已完成自主適應 AI；觀察、動作、評估證據與 owner 仍待全隊界定。

目前只有「咏宸已完成 UI」的使用者回報，尚未技術驗證。本次 PRD 規格決定已獲使用者授權，但尚未全隊共同確認、凍結或驗證。依本隊收到的主辦方最新決賽時程與回覆（收到日期待補；2026-09-10 同步），正式開發與提交為 10:35–17:35、共 7 小時，且賽前可換題、無須申請。正式展示時間與評分比重仍待確認，不得把 90 秒內部演練當成正式規則。

## 閱讀次序

1. 讀 [AGENTS.md](AGENTS.md) 的工作規則、[CONTEXT.md](CONTEXT.md) 的領域詞彙與 [ARCHITECTURE.md](ARCHITECTURE.md) 的責任及實作位置。
2. 讀 [PRD.md](PRD.md)：產品邊界、UI、後端契約、驗收標準；依本頁核對環境。
3. 讀 [ACCEPTANCE.md](ACCEPTANCE.md) 的完整案例與 [checklist.md](checklist.md) 的整體關卡。
4. 依 [WORKFLOW.md](WORKFLOW.md) 開工、處理 blocker、完成與交接；`/compact` 自行判斷，不是必經步驟。
5. 依下列時間安排完成串接、驗收與 90 秒內部演練。

文件定義「何者必須達成」與驗收案例。具體子 PRD、owner、工作順序、相依與 GitHub issue／Ticket 的拆分，均待團隊決定；本包不預先建立名稱、編號、連結或相依圖。

## 環境與從零啟動

Windows 為交接基準，Gradio UI 於 Windows 瀏覽器顯示，尚未實測。開賽後的實作 repo 共用 Python 3.11.16、`pyproject.toml` 與 `uv.lock`；每位隊員建立自己的 `.venv`。本 planning repo 不建立這些檔案或競賽碼。

既有指定版本待實測：`gradio==6.26.0`、`plotly==7.0.0`、`pytest==9.1.1`、`ruff==0.16.6`。不得自行升降版本或添加套件，也不得宣稱相容。首次建立實作環境時由工具產生 `uv.lock`；只有團隊確認依賴定義變更後才重新生成。平常以 `uv sync --locked` 檢查 lock 與定義一致，不更新 lock。若失敗，回報團隊。

以下命令僅在開賽後實作 repo 已存在相應入口與定義時使用；不可假裝目前已支援。

```powershell
# 首次建立 lock；僅團隊確認依賴定義變更後才重新執行
uv lock

# 每位隊員從共用 lock 建立自己的 .venv
uv sync --locked
```

啟動入口是交付要求，非既有能力假設：

```powershell
# 正式串接驗收：隊友提供可 import 的模組及 factory
uv run --locked python app.py --backend team --factory team_backend:create_backend

# 明確選擇的開發／備援示範
uv run --locked python app.py --backend local
```

`team_backend:create_backend` 是約定示例，可用 `--factory` 指定實際模組與函式。隊友必須把模組放在專案可 import 的位置，或提供其安裝方式。`--backend` 必填；team 模式缺 factory 或載入失敗時要明確報錯，絕不自動改為 local。

驗收工具入口存在時使用：

```powershell
uv run --locked python -m pytest -q --backend local
uv run --locked python -m pytest -q --backend team --factory team_backend:create_backend
uv run --locked python -m ruff check .
uv run --locked python -m ruff format --check .
```

`uv` 的 project layout、lock／sync、run 行為見官方文件：[layout](https://docs.astral.sh/uv/concepts/projects/layout/)、[sync](https://docs.astral.sh/uv/concepts/projects/sync/)、[run](https://docs.astral.sh/uv/concepts/projects/run/)。Gradio Quickstart 說明本機 UI 在瀏覽器顯示：[Quickstart](https://www.gradio.app/main/guides/quickstart)。

預設綁定 `127.0.0.1:7860`，不建立公開分享連結；允許 `--port` 改埠。每頁持續顯示「資料來源：隊友後端」或「資料來源：本機備援」，以及「Planner／Evaluator：MOCK 示範」。

<a id="開工與-7-小時安排"></a>

## 開工與 7 小時安排

最新已確認正式開發與提交窗口為 10:35–17:35，恰 7 小時。開工即確認隊友模組名稱、factory、依賴與一個可呼叫範例。未提供時，立即記錄阻礙；可繼續以 local 開發，但不可改變「必須接通 team」的完成條件。進度落後時，明確標示 blocked 並先減少裝飾與排版微調；不可刪除 tabs、動畫、策略、Mock 或必要驗收。

| 時間 | 工作 | 可確認的完成條件 |
| --- | --- | --- |
| 0–1 小時 | 環境、隊友 factory、HTML 動畫最小切片 | team 模式 `reset`、`snapshot`、`advance` 可呼叫；畫面顯示回傳狀態。 |
| 1–2.5 小時 | 契約凍結、相容層、本機對照模型、核心測試 | 基本策略、邊界、事件與 metrics 在兩種 adapter 通過；明列隊友接口差異。 |
| 2.5–4.5 小時 | Arena、控制項、動畫、注入與 session 管理 | 以 team 資料播放、暫停、切換策略與注入；列與歷史正確。 |
| 4.5–6.5 小時 | Metrics & Code、Library、Mock 流程、錯誤恢復與文件 | 接受／拒絕、Hybrid、code、來源標示與復原完整串接。 |
| 6.5–7 小時 | 整體驗收、提交緩衝與 90 秒內部演練 | 必要驗收結果及阻礙已記錄；在 17:35 前完成提交，不延至截止後。 |

分工採動態認領：未認領工作可自行登記；已有處理者或同檔修改時先協調。實際拆分、owner、工作順序與相依，須由團隊在開工時決定。

作者可先自驗並記「待共同確認」；實測符合預期、至少另一隊員共同確認、審查通過且整份子 PRD 合併後才通過。案例不另設批准流程。合併與發布權限不變；所有子 PRD 通過後仍須在共同 main 做整體串接及全隊確認最終展示。

## 90 秒展示

排練固定注入的模擬時間及操作序列，確保切換後仍有可派工訂單。此要求是展示可重現性；不要求與原影片使用同一批訂單或得出相同數值。

| 展示時間 | 操作與說明 |
| --- | --- |
| 0–15 秒 | 顯示「資料來源：隊友後端」及 Mock 標籤，重設並播放。說明一個 worker、三種方塊狀態。 |
| 15–30 秒 | Flash Sale 加四筆；說明等待帶表示期限消耗，不可行工作會留到截止才回收。 |
| 30–45 秒 | 提出 Mock Hybrid 候選；說明預寫規則且未經 evaluator 驗證，再按接受。 |
| 45–60 秒 | 觀察下一次派工使用 Hybrid；目前工作不中斷。必要時暫停後單步。 |
| 60–75 秒 | 暫停，切 Metrics & Code；展示實際 code、差異及累計指標。不宣稱 Hybrid 必然改善。 |
| 75–90 秒 | 開啟 Skill Library 查看 Hybrid；說明預覽與套用差別，完成展示。 |

## 給 AI 實作者的交付指令

依 [PRD.md](PRD.md) 從空專案實作，不需要原始碼或影片。先取得 team factory 並完成最小串接，再建立共用契約測試與本機對照 adapter，最後依里程碑完成 UI。不要先把所有畫面綁死本機 simulator，最後才嘗試串接。

交付說明必須列出：

- 安裝與啟動方式。
- 實際後端 factory。
- 資料生成規則。
- 已執行的驗收及結果。
- 仍有阻礙的項目。

隊友後端未就緒時，清楚區分「本機 demo 可用」與「team 串接未完成」。不可用程式行數、原專案測試數量、影片中的六件完成／兩件逾期，或像素級外觀相同當成功標準；只以 PRD 的行為、串接與驗收條件判定。

## 使用界線

- 開賽後才由本包建立實作 repo；規劃 repo 不輸出或累積參賽程式碼。
- 每位參與者使用自己的憑證；共享 GPU 不代表可共享憑證。
- 實作完成必須驗證 team 後端；不得以 local 結果冒充隊友後端結果。
- 已確認賽前可換題且無須申請；仍待確認的正式展示時間與評分比重不可自行推定。
