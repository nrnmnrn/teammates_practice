# 驗收案例

母 [PRD](PRD.md) 全文是唯一產品規則。本檔保留原有驗收案例與需求對照；`P01`–`P05` 僅為 legacy（舊有）案例分類與既有連結錨點，不是已核准的子 PRD、owner、Ticket、工作順序或相依圖。驗收案例不代表功能已實作或已通過；具體拆分與追蹤方式待團隊決定。

## 共通規則

- 作者可自驗；實測符合預期、至少另一隊員共同確認、審查通過且完整變更合併後，整體才可記為通過。
- 案例是審查依據，不另設案例勾選或批准流程；附件、截圖或影音由團隊決定，非強制。
- 必要案例失敗，整體未通過；範圍變更須團隊明確決定並更新規格。
- 修改後重驗受影響案例與串接；最終展示前重跑完整主流程。

## P01：啟動、重設、FIFO 與生命週期

**案例**：

- 正常：分別以 team 與明確選擇的 local 啟動；reset 後 t=0、FIFO、八筆初始資料及來源標籤正確，且 t=0 全部事件已處理，可已有 running、新 events 與 uses。播放與單步後，scheduled、pending、running、completed／expired、worker 和四張指標卡皆對應快照。
- 邊界：t=0 已到達且可行工作可派工；恰於 deadline 完成仍成功；全局結束後繼續播放，時間前進且吞吐量可下降。以同一 backend 比較 `factory(seed=99)` 的預設場景，與 `initial_jobs=[]` 建立後 `reset(seed=99)` 的八筆資料及後續相同 `generate(1)` 結果（排除 run_id 等識別）；驗證 reset 不沿用空測試場景且 seed 用於後續生成，不要求不同 backend 或不同 seed 產生相同資料。1440×900 與 1280×720 下控制項可用、無整頁水平溢出。
- 失敗：team 缺 factory 或載入失敗時，顯示明確錯誤；不得改用 local 或捏造快照。

## P02：注入、容量與輸入拒絕

**案例**：

- 正常：在剩餘名額足夠時注入 1 與 4 筆；當下到達資料立即依共用事件順序參與派工，畫面與 capacity 同步。
- 邊界：總數 20 時拒絕；剩餘 3 名額時整批 4 筆拒絕；completed／expired 不釋出名額。
- 失敗：重複 ID、過去 arrival、無效數值、bool、零／負工時、`deadline <= arrival` 或無效 count，均明確拒絕，jobs、RNG、events、時間無改變。

## P03：策略、Library 與 Mock 候選

**案例**：

- 正常：以 §6.1 A 驗 FIFO、SJF、Priority、EDF、Hybrid 首筆；Library 預覽不切換，套用才切換。idle、accepted、rejected 可提出單一 Mock Hybrid；接受後 Hybrid 加入兩處選單並登錄及更新策略快照；使用紀錄只隨實際派工更新。
- 邊界：arrival／ID 平手與 Hybrid priority、deadline、工時逐級比較；running 工作不被切換中斷；Arena／Library 共用切換立即影響下次派工；手動切換保留待決候選；相同策略 no-op 保留 reason／diff；重複登錄不產生重複 skill，已是 Hybrid 接受仍記候選事件但不新增 policy_changed／diff。
- 失敗：proposed 再提出、非 proposed 接受／拒絕、未知 policy 明確拒絕且不改狀態；拒絕維持操作當下策略與技能庫；reset 移除 Hybrid 與相關歷史。

## P04：Metrics & Code 觀察

**案例**：

- 正常：暫停後可讀同一 snapshot 的卡片、三圖、code、diff，並與 Arena 的策略／時間一致；§6.1 C 的 completed、expired、throughput、P95 與歷史趨勢正確。
- 邊界：t=0 throughput 為「—」；正時間無完成為零；空 P95 為「—」；單筆與多筆 P95 正確；同時刻只留最終值，無事件推進不加永久取樣但目前點吞吐量更新；只以後端 metrics／series 按 time 去重後顯示最近 60 點（含目前點），且不把整局結果宣稱為新策略效果。
- 失敗：尚未切策略時顯示「尚無策略變更」；取得 code／同步失敗時依 P05 的錯誤規則保留最後有效畫面，不能混用舊新資料。

## P05：會話隔離、舊回應與錯誤恢復

**案例**：

- 正常：兩個瀏覽器 session 各自有 adapter；Arena 控制操作由單一序列化入口執行，成功後更新其 revision。
- 邊界：reset 後 generation 增加、revision 不歸零，舊 timer／回應不回灌；若 reset 成功後讀取失敗，重新同步發現新 run_id 仍可更新 generation 並發布完整新快照。snapshot 防禦性副本，重同步只讀取、不重送注入。
- 失敗：負 dt 與其他 ValidationError 完全保持原狀；可證明未修改的 OperationError 暫停、保留最後有效畫面，使用者可修正後再操作。不明狀態則鎖定所有修改，連已排隊操作在執行前也拒絕，只允許重新同步或 reset；完整 snapshot 與 skills 同步成功，或 reset 讀回成功後才解除鎖，仍暫停且不重送。team 失敗永不降為 local。

## 總體驗收與展示

在共同 main 進行最終全套整體驗收：local 與真 team 各跑完整契約 suite；team 模式跑完 90 秒內部流程；全隊共同確認最終展示。真正自主適應 AI 的觀察、動作、評估證據與 owner 尚待全隊界定，因此不可由 Mock 或本包驗收宣稱完成。任何必要案例未通過，整體仍未通過。

## 母 PRD 對照

下表只將 PRD 條款對照至 legacy 案例分類，不表示工作拆分或先後相依。

| 母 PRD 條款 | 驗收案例分類 |
| --- | --- |
| §§3–3.3 | P01、P02、P03 |
| §§4.1–4.3 | P01、P02、P03、P04 |
| §§5.1–5.7 | P01、P02、P04、P05 |
| §6.1 A–D | P01、P03、P04 |
| §6.2-1 FIFO 基本路徑 | P01 |
| §6.2-1 其餘策略與所有平手 | P03 |
| §6.2-2 不可行、閒置、deadline | P01 |
| §6.2-3 `advance` 分段一致 | P01 |
| §6.2-4 metrics 與 P95 | P04 |
| §6.2-5 seed 與失敗注入 | P02 |
| §6.2-6 容量 | P02 |
| §6.2-7 JobInput、批次與 count | P02 |
| §6.2-7 未知 policy | P03 |
| §6.2-7 負 dt 與狀態不變 | P05 |
| §6.2-8 Mock 與 reset | P03 |
| §6.2-9 Arena／Library 與非搶占 | P03 |
| §6.2-10 factory、snapshot、序列化 | P01、P05 |
| §6.2-11 controller 與舊回應 | P05 |
| §6.2-12 API 型別與錯誤恢復 | P02、P05 |
| §6.2-13 真 team 與無 fallback | P01、P05、總體驗收 |
| §6.3-1 三個 tabs 可使用、標籤始終可見 | 總體驗收 |
| §6.3-2 狀態、歷史 | P01 |
| §6.3-3 控制、容量 | P01、P02 |
| §6.3-3 切 tab 不重複推進 | P04、總體驗收 |
| §6.3-4 20 筆、捲動、版面 | P01、P02 |
| §6.3-5 策略、code、diff、圖表同步 | P03、P04、總體驗收 |
| §6.3-6 暫停與全局結束 | P01、P04 |
| §6.3-7 失敗 adapter 與無降級 | P05 |
| §6.3-8 真 team 90 秒流程 | 總體驗收 |
