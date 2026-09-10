# 環境與 session 習慣

本頁記錄團隊可採用的賽前環境原則與個人經驗。正式實作規則以未來交接包的 [README](../../build-handoff/README.md)、[AGENTS](../../build-handoff/AGENTS.md) 與 [WORKFLOW](../../build-handoff/WORKFLOW.md) 為準。

## 賽前環境

- 開賽後，由完整 `build-handoff/` 建立實作 repo；不要只複製其中一份模板。現有入口文件彼此以相對路徑引用；子 PRD 目錄待拆分計畫獲團隊核可後才建立。
- `AGENTS.md` 是實作 repo 專用規則，不要把整份覆蓋到全域 `~/.codex/AGENTS.md`。個人可在全域檔保留跨專案都適用的短規則；repo 規則留在 repo 內。
- 依實作 repo 的 `pyproject.toml` 與 lockfile 使用 `uv`。先讀既有設定，再依交接包執行 `uv sync --locked`、測試、lint 或型別檢查；不要為了本流程全域安裝 `ruff`、`pytest` 或 `mypy`，也不要手改依賴或 lockfile。
- 安裝並啟用真正需要的 skills／plugins 即可。每個額外工具都會增加選擇與 context 負擔；適用情境見 [Skills 情境對照表](skill-routing.md)。
- 賽前演練只在可丟棄的 toy／sandbox repo 進行，不能產生可搬入競賽的程式碼。憑證各自管理，不共享 API key、token、密碼或 `.env`。

## 一張 Ticket 的工作順序

1. 讀交接包與當前 Ticket，確認成功條件、依賴、範圍和已存在的未提交修改。
2. 依 Ticket 執行實作、相關測試與 `/code-review`；程式變更依 repo 規則執行檢查。
3. 實作、測試與 review 均完成後，執行 `$task-closeout` 整理規格、證據、Git 狀態與下一步，並請求人類批准 commit。
4. 未獲批准不 commit、push、merge 或 deploy；已有明確批准時，依該範圍執行，不重複詢問。

`/compact` 由當前 agent 依對話長度與可讀性判斷；不是任何步驟的必要前置。換 session 或工作 Blocked 時可用 `$handoff`；它只寫 OS 暫存區，僅摘要既有文件尚未記錄的新進度、假設與 blocker，並引用現成規格、issue、測試、review 或 diff。不是每張 Ticket 都必須 handoff，也不能把未完成工作寫成完成。

## 個人觀察／待驗證；非強制流程

以下是過去使用經驗，不是 Codex 或比賽的已確認規則：

- 有人曾在 Codex CLI 成功使用某 MCP，而 desktop UI 當時沒有成功；遇到工具差異時，先記錄版本、操作與錯誤，再用目前環境重現或求助。
- 結束一段工作或需要另開脈絡時，可自行選擇 `/quit`、新 terminal、`/fork` 或 `/compact`。使用前以目前 CLI 的說明與實際結果為準；不要把舊經驗視為固定 bug 或必做程序。

## 白話說明

交接包像一套同款零件的說明書；只拿走其中一頁，其他頁的頁碼會對不上。全域規則則像個人筆記，應只放人人通用的提醒，不能取代某一場比賽的施工圖。每張工作票先完成、測過、審過，再請人決定是否提交；交接只是在換人時留下便條，不是每次都必須寫。
