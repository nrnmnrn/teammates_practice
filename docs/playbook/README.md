# Hackathon Vibe Coding 操作手冊

本手冊提供賽前規劃與開賽後實作的共同流程。比賽資訊、產品決定和程式實作分開管理；開賽前不在本 repo 或演練 repo 建立可搬用的競賽程式碼。

## 文件入口

1. [基本觀念](concepts.md)：共同名詞與協作原則。
2. [賽前安裝與檢查](setup.md)、[環境與 session 習慣](basic_workflow_and_environment_setting.md)：安裝檢查、交接包、`uv`、個人觀察與演練界線。
3. [子 PRD 工作流程](workflow.md)：認領、實作、驗證、closeout 與交接。
4. [Skills 情境對照表](skill-routing.md)：何時選擇流程。
5. [故障與降級處理](troubleshooting.md) 與 [Codex 實務經驗](codex-practices.md)：卡住時的證據與待驗證經驗。

未來實作 repo 的唯一文件介面是完整 [實作交接包](../../build-handoff/README.md)，不是本手冊的局部複製。

## 子 PRD、issue 與 Ticket

- 總 PRD 定義產品；一份子 PRD 是可獨立驗收、可合併的完整功能；Ticket 是該子 PRD 內一次可驗收的工作。
- GitHub 父 issue 記錄子 PRD 的認領、狀態與證據；子 issue 記錄 Ticket。文件的依賴為準，issue 連結只是鏡像。
- 一份子 PRD 使用一條 branch 與一個 Draft PR。通常包含二張、最多三張 Ticket；每張 Ticket 原則一個 commit。
- 全部 Ticket、必要驗收、完整測試、AI review、作者自查與至少另一位隊員共同確認完成後，才可請有權限者合併子 PRD，並在 `main` 驗證整合。每張 Ticket 不另開 PR 或單獨 merge。

## 團隊規則

- 開工先讀完整子 PRD、當前 Ticket、前置證據與 `main`；已有認領或同檔修改先協調。領域沒有固定 owner，編號不代表工作順序。
- 行為改變要有相關測試；提交前閱讀 diff。保留完整錯誤原文，不以猜測改寫。
- 依 [工作流程](workflow.md) 先完成實作、測試與 review，再用 `$task-closeout` 整理並請求 commit 批准。已有明確批准時不重問；未獲批准不得 commit、push、merge 或 deploy。
- `/compact` 自行判斷；換 session 或 Blocked 時可用 `$handoff` 記錄新進度與 blocker，不是每張 Ticket 的固定流程。
- API key、SSH private key、密碼、token 與 `.env` 不可放入聊天、prompt、issue 或 repo。賽前僅在可丟棄的 toy／sandbox repo 演練，且不得放競賽碼。

## 白話說明

子 PRD 像一間完整房間，Ticket 像其中幾次施工。房間共用一條施工路線和一份施工申請，不是每裝一顆螺絲就另開工地。施工完成、檢查與同伴確認後，才由有權限的人決定是否交付。
