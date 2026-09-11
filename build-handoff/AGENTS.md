# 實作 repo 指引

本包複製至實作 repo 後才是工作基線；規劃 repo 仍只保存規劃文件。開始任何工作，先讀 [INDEXER.md](INDEXER.md)，依觸發情境只載入必要文件。若目前位於非 `main` branch，代表正在承接一份 sub-PRD；修改前必須再讀 [workflow.md](workflow.md)，並核對已核可 sub-PRD、GitHub parent issue 與目前 Ticket。

PRD 是產品需求權威；已核可 sub-PRD 是該功能範圍權威；GitHub parent issue 是實際執行狀態與角色指派權威。產品決策仍在主辦方回覆題目變更請求前暫停。

只使用個人憑證；不得把憑證寫入 repo、issue 或 prompt。新增依賴、schema、CI/CD、authentication 或 authorization 變更前先取得核可；不得削弱驗證以通過測試。使用 uv 與專案 `.venv`。

完成一張 Ticket 前，依 INDEXER 所指文件留下驗證證據；不得自動 push、merge 或 deploy。
