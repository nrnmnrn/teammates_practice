# 一份 sub-PRD 一條 branch

> Status: accepted

## 決策

一份 sub-PRD 對應一條 branch 與一個 Pull Request；該 sub-PRD 的所有 Tickets 都在這條 branch 完成。在實作 repo，非 `main` branch 必須對應一份 sub-PRD。開始修改前，工作者核對 GitHub parent issue、已核可規格與目前 Ticket。

branch 表示工作歸屬；實際狀態仍以 GitHub parent issue 為準。parent issue 記錄實際 branch 與 Pull Request 連結；本 ADR 不追蹤執行狀態。

## 取捨

相較於每張 Ticket 一條 branch，此決策減少反覆開 PR、合併、rebase 與跨 Ticket 整合成本，也讓整份功能以一個可審核變更集交付。

代價是同一條 branch 的工作須協調順序、避免同檔衝突，並在每張 Ticket 後保留可驗證證據。若 sub-PRD 大到無法承受此協調成本，應先重切 sub-PRD，而不是改用多條 branch。
