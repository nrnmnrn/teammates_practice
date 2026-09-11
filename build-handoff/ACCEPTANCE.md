# 實作交接包驗收

本檔記錄驗收狀態與證據，不改寫 PRD。每項只驗證一件事；勾選時填入結果、證據位置／命令、日期與審核者。完整交付必須以 team backend 通過驗收；local backend 可明確選作開發或備援。team 啟動失敗時顯示錯誤，不會自動切換 local，也不能以 local 結果宣稱 team 已通過。

規格版本填驗證／凍結所用 canonical Git commit。主辦方題目變更回覆、賽事規則與團隊凍結尚未確認；核對前維持產品決策暫停。

| ID | 檢查項目 | PRD 段落 | 驗證方法 | 適用 backend | 結果 | 證據 | 審核者 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| D1 | team 模式可由指定 factory 啟動。 | §1.2、§5.1 | 以 `--backend team --factory` 啟動並保存輸出。 | team | 待驗 | 待填 | 待填 |
| D2 | local 模式只在明確選擇時啟動。 | §1.2、§5.6 | 以 `--backend local` 啟動並確認來源標籤。 | local | 待驗 | 待填 | 待填 |
| D3 | team 載入失敗顯示錯誤且不 fallback。 | §5.1、§5.6、§6.2 | 以缺 factory 或失敗 factory 啟動，確認無 local 快照。 | team | 待驗 | 待填 | 待填 |
| J1 | Job 輸入與生命週期符合契約。 | §3.1、§3.2、§5.3 | 契約測試覆蓋有效、無效與每個狀態轉換。 | team、local | 待驗 | 待填 | 待填 |
| J2 | Run reset 清除並還原規定狀態。 | §3.3、§5.2 | 契約測試驗證時間、Job、Policy、產品 Skill 與歷史。 | team、local | 待驗 | 待填 | 待填 |
| P1 | FIFO 依定義選擇下一筆 Job。 | §3.2、§6.1 A | 以共同資料案例 A 驗證。 | team、local | 待驗 | 待填 | 待填 |
| P2 | SJF 依定義選擇下一筆 Job。 | §3.2、§6.1 A | 以共同資料案例 A 驗證。 | team、local | 待驗 | 待填 | 待填 |
| P3 | Priority 依定義選擇下一筆 Job。 | §3.2、§6.1 A | 以共同資料案例 A 驗證。 | team、local | 待驗 | 待填 | 待填 |
| P4 | EDF 依定義選擇下一筆 Job。 | §3.2、§6.1 A | 以共同資料案例 A 驗證。 | team、local | 待驗 | 待填 | 待填 |
| P5 | Hybrid 依定義選擇下一筆 Job。 | §3.2、§6.1 A | 接受 Mock proposal 後以共同資料案例 A 驗證。 | team、local | 待驗 | 待填 | 待填 |
| P6 | deadline 邊界與同時刻事件順序正確。 | §3.2、§6.1 B | 以共同資料案例 B 驗證事件與終態。 | team、local | 待驗 | 待填 | 待填 |
| P7 | 不同 advance 分段產生相同結果。 | §3.2、§6.2 | 比較大步與小數分段的 Job、事件與 metrics。 | team、local | 待驗 | 待填 | 待填 |
| A1 | Scheduling Arena 顯示後端權威狀態。 | §4.1 | 瀏覽器驗收灰／綠／黃、歷史、worker 與所有 Job。 | team | 待驗 | 待填 | 待填 |
| A2 | Arena 控制項不重複推進或超出名額。 | §3.3、§4.1 | 瀏覽器操作播放、暫停、單步、倍速、reset、注入與切 tab。 | team | 待驗 | 待填 | 待填 |
| A3 | Arena 在指定桌面尺寸可讀且無水平溢出。 | §4.1 | 在 1440×900 與 1280×720 截圖檢查。 | team | 待驗 | 待填 | 待填 |
| M1 | metrics 計算符合定義。 | §4.2、§6.1 C | 以共同資料案例 C 比較原始值。 | team、local | 待驗 | 待填 | 待填 |
| M2 | Metrics & Code 同步同一份快照。 | §4.2、§5.6 | 瀏覽器操作切換 Policy 後核對卡片、圖表、code 與 diff。 | team | 待驗 | 待填 | 待填 |
| S1 | 產品 Skill 預覽不改變 Policy。 | §4.3 | 在 Skill Library 預覽後比對快照 Policy。 | team、local | 待驗 | 待填 | 待填 |
| S2 | 套用產品 Skill 依共用切換流程生效。 | §4.3、§5.2 | 從 Arena 與 Library 各套用一次並比對事件。 | team、local | 待驗 | 待填 | 待填 |
| S3 | Mock proposal 的接受與拒絕符合狀態機。 | §1.3、§4.3、§5.2 | 契約測試驗證 idle、proposed、accepted、rejected 與 reset。 | team、local | 待驗 | 待填 | 待填 |
| B1 | adapter factory 與 Snapshot 契約完整。 | §5.1–§5.5 | 共用契約測試覆蓋 factory、公開方法、JSON 輸出與兩個實例。 | team、local | 待驗 | 待填 | 待填 |
| B2 | session 拒收舊回應。 | §5.6 | controller 測試驗證 generation、run_id、revision。 | team、local | 待驗 | 待填 | 待填 |
| B3 | 修改失敗後的錯誤恢復符合契約。 | §5.6–§5.7 | 以可控制失敗 adapter 驗證暫停、保留畫面、resync／reset。 | team、local | 待驗 | 待填 | 待填 |
| T1 | 自動測試通過並標示 backend 來源。 | §6.2 | 分別執行 local 與 team 契約測試，保存完整結果。 | team、local | 待驗 | 待填 | 待填 |
| T2 | lint 與 format check 通過。 | §1.2、§6.2 | 執行 PRD 指定 Ruff 命令。 | 實作 repo | 待驗 | 待填 | 待填 |
| X1 | 90 秒展示可在 team 模式重複完成。 | §6.3、§8 | 依 README 腳本錄操作、輸出與必要截圖。 | team | 待驗 | 待填 | 待填 |
| F1 | 文件包不含越界內容或 secrets。 | §1.2、README「使用界線」 | 人工檢查交接包內容與來源。 | 文件包 | 待驗 | 待填 | 待填 |
| F2 | 賽前演練可依交接包重啟。 | §7、README「開工與 7–8 小時安排」 | 於獨立可丟棄 sandbox repo 由新手演練並記錄結果。 | sandbox | 待驗 | 待填 | 待填 |
| F3 | 正式凍結已取得主辦方回覆與團隊確認。 | §1.2、README「使用界線」 | 附主辦方回覆、團隊確認與凍結紀錄。 | 文件包 | 待驗 | 待填 | 待填 |

凍結日期／時間：待填

審核者：待填

Rehearsal repo：待填
證據位置：待填
