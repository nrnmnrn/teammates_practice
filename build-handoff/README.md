# 實作交接包

此資料夾是規劃 repo 與未來比賽實作 repo 的唯一文件介面。唯一自足的產品規格是 [PRD.md](PRD.md)；`checklist.md` 記錄執行狀態與可檢查證據。`README.md`、`PRD.md` 與 `checklist.md` 三檔必須一起帶入新的實作 repo，且本資料夾內所有連結可獨立使用，不依賴規劃 repo 其他檔案。

使用者已授權把原始 PRD 正式遷入本交接包。規格版本應記錄驗證／凍結實際使用的 canonical Git commit；目前待提交後填入。`088ae4cdf7c9f6cbb439a70630bfff60c49d09d9` 只可作原根目錄完整 PRD 的歷史來源，不是本次遷移後的 canonical 版本。不要再將根目錄檔案、複製雜湊或 capture 日期當成權威。根目錄只作導向入口。

本包描述 Gradio 排程展示：三個 tabs、五種固定策略、真實排程與指標，以及 Mock（預寫示範，非真實 AI）候選流程。隊友後端是完整交付必要條件；`local` simulator 僅可在明確選擇時作開發或備援展示。`team` 後端載入失敗必須明確失敗，不得靜默改為本機資料。

目前只有「咏宸已完成 UI」的使用者回報，尚未技術驗證。此遷移已獲授權；只有仍依賴主辦方回覆題目變更請求的產品決策維持暫停。規劃日期、賽事規則及團隊凍結均未確認，不得當成已確認事實。

## 閱讀次序

1. 讀 [PRD.md](PRD.md)：產品邊界、領域規則、UI、後端契約、驗收標準。
2. 依本頁完成環境與開工檢查。
3. 用 [checklist.md](checklist.md) 記錄每個 PRD 驗收標準的責任、狀態與證據。
4. 依 PRD 的 7–8 小時安排完成串接、驗收與 90 秒展示。

PRD 定義「何者必須達成」及接受標準；checklist 只記錄「誰已用何證據證實何狀態」，不另改寫或放寬 PRD。

## 環境與從零啟動

目標環境是 Windows、Conda `scheduler-ui` 與 Python 3.11.16。使用 uv 安裝套件，且要明確指定 Conda 環境的 Python；不要另建專案 `.venv`。

`requirements.txt` 必須固定包含：

```text
gradio==6.26.0
plotly==7.0.0
pytest==9.1.1
ruff==0.16.6
```

在已可使用 Conda、uv 的 PowerShell 或 Anaconda PowerShell Prompt 執行：

```powershell
conda create -n scheduler-ui python=3.11.16 pip -y
conda activate scheduler-ui
uv pip install --python "$env:CONDA_PREFIX\python.exe" -r requirements.txt
uv pip check --python "$env:CONDA_PREFIX\python.exe"
```

若環境已存在，跳過建立步驟。專案指令不得寫死原開發者帳號或絕對路徑。

必須提供以下啟動入口；它們是交付要求，不能假設舊專案已支援：

```powershell
# 正式串接驗收：隊友提供可 import 的模組及 factory
python app.py --backend team --factory team_backend:create_backend

# 明確選擇的開發／備援示範
python app.py --backend local
```

`team_backend:create_backend` 是約定示例，可用 `--factory` 指定實際模組與函式。隊友必須把模組放在專案可 import 的位置，或提供其安裝方式。`--backend` 必填；team 模式缺 factory 或載入失敗時要明確報錯，絕不自動改為 local。

預設綁定 `127.0.0.1:7860`，不建立公開分享連結；允許 `--port` 改埠。每頁持續顯示「資料來源：隊友後端」或「資料來源：本機備援」，以及「Planner／Evaluator：MOCK 示範」。

<a id="開工與-7-8-小時安排"></a>

## 開工與 7–8 小時安排

開工即確認隊友模組名稱、factory、依賴與一個可呼叫範例。未提供時，立即記錄阻礙；可繼續以 local 開發，但不可改變「必須接通 team」的完成條件。進度落後時，先減少裝飾與排版微調；不可刪除 tabs、動畫、策略、Mock 或必要驗收。

| 時間 | 工作 | 可確認的完成條件 |
| --- | --- | --- |
| 0–1 小時 | 環境、隊友 factory、HTML 動畫最小切片 | team 模式 `reset`、`snapshot`、`advance` 可呼叫；畫面顯示回傳狀態。 |
| 1–2.5 小時 | 契約凍結、相容層、本機對照模型、核心測試 | 基本策略、邊界、事件與 metrics 在兩種 adapter 通過；明列隊友接口差異。 |
| 2.5–4.5 小時 | Arena、控制項、動畫、注入與 session 管理 | 以 team 資料播放、暫停、切換策略與注入；列與歷史正確。 |
| 4.5–6 小時 | Metrics & Code、Library、Mock 流程 | 接受／拒絕、Hybrid、code 與來源標示完整串接。 |
| 6–8 小時 | 全面驗收、錯誤恢復、文件、demo 排練 | pytest／Ruff 與瀏覽器驗收完成，90 秒展示走通。 |

角色分工：開發者負責 UI、controller、相容層、測試與整合；隊友負責可 import 的完整排程後端、契約能力及後端缺陷修正；AI 依里程碑實作、驗證並回報問題。

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
- 主辦方回覆前，不新增仍依賴該回覆的產品決策，亦不推定賽事規則。
