# sub-PRD 撰寫與核可

sub-PRD 是一項完整功能的權威規格。草稿須用 [模板](templates/sub-prd.md)，並連回 GitHub parent issue；規格不複製易變的執行狀態。

## 權限

| 動作 | 可執行者 | 生效條件 |
| --- | --- | --- |
| 建立、維護草稿 | owner | 可直接進行。 |
| 修正文句、連結、證據或不改變核可內容的說明 | owner | 可直接進行，保留版本紀錄。 |
| 補充原範圍遺漏的 Ticket | owner | 記錄理由；一份 sub-PRD 最多 5 張 Ticket。 |
| 改變交付結果、範圍、驗收、跨 sub-PRD 依賴或 owner | main branch 負責人 | main branch 負責人核可後才生效。 |
| 變更已核可規格 | owner 提案、main branch 負責人核可 | 核可前只能是提案版本，不得取代執行基準。 |

草稿至少有交付結果、範圍／不做範圍、PRD 關係、依賴與理由、驗收、owner、GitHub parent issue 與未解問題。未知的交付結果、範圍、驗收或依賴是 blocker，不可核可。

一份 sub-PRD 通常有 2–3 張 Ticket，最多 5 張。超過上限時，由 main branch 負責人重評。實際狀態、角色指派、核可連結、規格版本與證據記在 GitHub parent issue；功能依賴及其理由保留在 sub-PRD。
