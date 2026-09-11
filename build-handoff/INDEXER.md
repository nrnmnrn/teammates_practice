# 交接索引

先用下表選讀；不預設載入全部文件。

| 觸發情境 | 應讀文件 | 讀完必須取得的答案 |
| --- | --- | --- |
| 開始任何實作、驗收或文件工作 | [AGENTS.md](AGENTS.md)、本檔 | 此次工作界線、權威來源與還要讀什麼？ |
| 判斷產品行為、範圍或完成條件 | [PRD.md](PRD.md)、[CONTEXT.md](CONTEXT.md)、[ACCEPTANCE.md](ACCEPTANCE.md) | 要交付什麼、名詞是什麼、如何證明完成？ |
| 建立、修改或核可 sub-PRD | [sub-prd-authoring.md](sub-prd-authoring.md)、[templates/sub-prd.md](templates/sub-prd.md)、[docs/adr/0001-document-authority.md](docs/adr/0001-document-authority.md)、[docs/adr/0002-one-branch-per-sub-prd.md](docs/adr/0002-one-branch-per-sub-prd.md) | 誰能改什麼、規格如何連 issue、branch 如何歸屬？ |
| 開始或續做 Ticket | [workflow.md](workflow.md)、已核可 sub-PRD、GitHub parent issue、目前 Ticket | 此 Ticket 的核可範圍、前置條件、branch／PR 與可驗證結果？ |
| backend、session、錯誤或 UI 行為 | [PRD.md](PRD.md) 第 4–6 節、[ACCEPTANCE.md](ACCEPTANCE.md) | 適用 backend、契約、復原行為與證據方法？ |
| 調整 agent 指引、流程或模板 | [workflow.md](workflow.md)、本檔、[docs/adr/](docs/adr/) | 哪份文件是唯一權威，變更是否改到既定決策？ |
| 準備整合、凍結或展示 | [workflow.md](workflow.md)、[ACCEPTANCE.md](ACCEPTANCE.md)、GitHub parent issue | 尚缺哪些證據、誰要核可、何時才可標示完成？ |

程式尚未建立，故不預設目錄或模組名稱。第一張已核可 Ticket 決定測試位置與 team backend 的可 import 位置，並在 sub-PRD 記錄理由。PRD 要求 `app.py` 為啟動入口；`team_backend:create_backend` 只是可由 `--factory` 取代的示例。
