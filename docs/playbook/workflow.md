# 子 PRD 工作流程

完整協作規則見[四人協作政策](../operations/team-work-allocation.md)；skill 的適用情境見[技能對照表](skill-routing.md)。總 PRD 定義產品，子 PRD 定義可合併的完整功能，Ticket 定義一次可驗收工作。

## 認領與開始

- 認領未認領、依賴已就緒的工作；領域沒有固定 owner。已有認領或同檔修改時，先協調。
- 于喬保留既有 merge 授權。認領不改變合併、發布或產品決定的權限。
- 子 PRD 的實際切分、名稱、依賴、owner 與 issues 均待團隊核可後才建立。核可後，規格文件管規格和依賴；GitHub 父 issue 管子 PRD 的認領、狀態與證據，子 issue 管 Ticket。issue 的依賴連結只是文件依賴的鏡像；開工前確認父子 issues 已建立並引用核可規格，尚未建立時僅在拆分計畫已核可且具建立任務的授權時建立，不捏造編號。
- 每份子 PRD 使用一條 branch 與一個 Draft PR。每張 Ticket 原則一個 commit；一次 agent session 一票。正常一份子 PRD 先排二張、最多三張 Ticket；超過即重檢切分，不能用巨票遺漏驗收。

開工前做短檢查：讀總 PRD、子 PRD、當前 Ticket、相依證據、最新 `main` 與未解紀錄。跨子 PRD 的前置成果必須已 merge 並在 `main` 驗證，不能只看 issue closed；于喬明確同意穩定介面並行並留下證據時例外。同一子 PRD 的下一張 Ticket 可在同 branch 使用前一張成果。資訊不足、互相矛盾或無法判斷卡點，使用 `$caveman:investigate-first` 收集證據；需要產品取捨時，由人決定。原因不明、難重現或偶發程式錯誤使用 `$diagnosing-bugs`。已清楚的 Ticket 用 `$implement`；`$caveman:cavecrew` 只用於定位、至多兩檔小修或精簡 diff review，不保證 token 節省或結果正確。

## 執行與完成

1. 只做一張可執行 Ticket，依真實依賴順序，不只看票號。
2. 執行 `$implement`，完成實作、相關測試與 `/code-review`。
3. 實作、必要測試與 review 通過後，執行 `$task-closeout` 核對規格、證據、Git 狀態與下一步，並申請 commit 批准。
4. 本團隊覆寫 `$implement` 的自動 commit：**未取得明確授權不得 commit。**可複製指令：`請依本 Ticket 執行 $implement，完成實作、測試與 /code-review；不要 commit。必要驗證通過後，執行 $task-closeout 提出核可。`已有授權時依其範圍，不重複詢問。
5. closeout 完成後，可問是否使用 `$handoff`。換 session 或 Blocked 也可 handoff；不可把未完成工作寫成完成。handoff 只寫 OS 暫存目錄，不進 repo；摘要文件未記錄的進度、假設與 blocker，並引用既有規格、issue、測試、review 與 diff 證據。

`/compact` 完全由當前 agent 自行判斷；不是開工、closeout、handoff 或 commit 的前置條件。

同一 blocker 超過 15 分鐘，或兩次有證據嘗試仍未解，標為 Blocked 並回報。完成一份子 PRD 前，全部 Ticket、必要驗收、完整測試、AI review 與作者自查都要具備；作者自驗後，至少另一位隊員共同確認，於父 issue／PR 留下驗證版本、預期與實際結果及審查證據。merge 與 release 仍須使用者授權；merge 後還要在 `main` 確認整合結果。日期與參與者沿用 GitHub 紀錄，不在目錄另填欄位。

## 交接包與環境

目前交接包的短版實作流程見 [WORKFLOW.md](../../build-handoff/WORKFLOW.md)。日後子 PRD 目錄只在拆分計畫獲團隊核可後建立；本 planning repo 不寫比賽程式。

產品規格與範圍需團隊決定；現有已確認與待確認比賽資訊以[比賽資訊](../competition/README.md)為準。實作 repo 使用既有 `uv` 設定；先檢查既有配置，不新增依賴。

賽前只在獨立、可丟棄的非比賽 toy／sandbox repo 演練流程，且不得放競賽碼或可重用實作。CI 須獲使用者明確授權才導入；CD 非預設。

## 白話說明

子 PRD 像一間完整房間，Ticket 像一次施工。先確認地基和前一段工程可用，再認領施工；每次只完成一小步並留下可查證結果。房間全部完成後，大家一起檢查，再由有權限的人合併。若圖紙不清或牆面出問題，先查清楚或請人決定，不能猜著施工。
