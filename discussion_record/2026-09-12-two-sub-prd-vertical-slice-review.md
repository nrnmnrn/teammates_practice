# 兩份主要子 PRD 與 vertical-slice Ticket 討論紀錄

- 日期：2026-09-12
- 性質：對話紀錄，供後續討論追蹤
- 權威限制：本文件不是總 PRD、子 PRD、Ticket、核准紀錄或流程規範。若與正式文件衝突，以正式文件及最新團隊決議為準。

## 本輪輸入

于喬提供一份依 `$to-tickets` 重新整理的建議，主張：

- 原先 Scheduler、Agent、Sandbox、Skill Library、Demo UI 五分法偏向技術責任區，可能造成水平切割。
- 正式子 PRD先收斂成 `scheduling-arena.md` 與 `adaptive-skill-lifecycle.md`。
- Demo UI 功能分配至兩份子 PRD；只有證明可獨立交付時才另建 `demo-experience.md`。
- 每份子 PRD通常拆 2～3 張 Ticket，最多 5 張。
- Ticket 應是可獨立展示或驗證、適合一個新 context window 的 vertical slice。
- AO-001 至 AO-010 維持草案，不作正式 Ticket。

于喬同意其餘既有整合方向。

## 已核對的規則來源

`build-handoff/sub-prd-authoring.md` 規定：

- 子 PRD 是一項完整功能的權威規格。
- 一份子 PRD通常有 2～3 張 Ticket。
- 最多 5 張；超過由 main branch 負責人重評。

`$to-tickets` 規定：

- 每張 Ticket 應形成跨層的窄而完整路徑。
- 完成後可獨立展示或驗證。
- 大小須適合一個全新 context window。
- 每張 Ticket 要列 blocking edges。
- 發布前先向使用者展示拆解，確認粒度、依賴及是否需要合併或再拆。
- 使用者核准後才發布至已設定的 tracker。

重要區分：2～3 張及最多 5 張是本 repo 的流程規則，不是 `$to-tickets` skill 自帶的上限。因此，final 必須保留本地規則；比賽當日使用 `$to-tickets` 時也要明示此限制。

## 對兩份主要子 PRD 的判斷

### `scheduling-arena.md`

此邊界合理。它交付一條無 Agent adaptation 仍可運作的完整路徑：

- workload 進入系統。
- Scheduler 使用既有 policies。
- 系統產生 job states、events 與 metrics。
- Gradio 顯示過程與結果。
- 使用者可以播放、暫停、單步、重設及改變 workload。

此功能能獨立展示及驗收，符合子 PRD 的完整功能定義。預估三張 Ticket 可作規模檢查，但正式拆解留待比賽當日。

### `adaptive-skill-lifecycle.md`

此邊界也合理。它以「系統從退化到安全啟用新 Skill」作一項使用者可觀察的完整能力，包含：

- 偵測 metrics 惡化。
- 優先重用既有 Skill。
- 必要時生成 Candidate。
- Sandbox 與 Evaluator gate。
- Critic feedback 及有限次重試。
- 註冊、啟用及 UI 可見證據。
- 失敗時保留最後有效狀態。

Agent、Sandbox、Skill Library 與相關 UI 在此是完成同一生命週期的技術責任，不須各自成為子 PRD。

此子 PRD接近五張 Ticket 上限。正式拆解時若任一 Ticket 無法在單一新 context window 完成，應先檢查 Ticket 是否太水平或子 PRD 是否過大，不可為符合數量而塞入超大 Ticket。

## Demo UI 判斷

不預設建立 `demo-ui.md` 或 `demo-experience.md`。

- 基礎排程 UI 屬 `scheduling-arena.md`。
- adaptation 的狀態、結果與錯誤呈現屬 `adaptive-skill-lifecycle.md`。
- 共用導覽、資料來源標示、Demo／Developer mode 與 fallback 若只是整合驗收，放入 `ACCEPTANCE.md`。
- 只有它們形成可獨立交付、獨立驗收、合理使用一個 branch 並需要 2～3 張 Ticket 的完整功能，才建立 `demo-experience.md`。

## 依賴建模的重要修正

兩份子 PRD 不宜只寫成簡單的完全串行關係。應分辨：

- 硬阻斷：未完成便不能開始工作的真實前置條件。
- Contract 依賴：介面先確定後，可用 adapter 或 deterministic fake 平行開發。
- 整合依賴：兩邊各自完成後，才可執行的共同驗收。

`adaptive-skill-lifecycle.md` 需要 Scheduler 的 Snapshot、events 與 metrics contract，但不一定要等整個 `scheduling-arena.md` 全部完成才開始。若流程要求依賴先進 main，則須在子 PRD 明載可用穩定 contract／adapter 的例外及驗證方式。

此區分應寫入 `sub-PRDs/DEPENDENCIES.md`，避免錯誤 blocking edge 使兩位成員無法平行。

## UI 重疊風險

兩份子 PRD 都會改 UI。為降低同檔衝突，正式規格須明定：

- `scheduling-arena.md` 建立共用 UI shell、基礎 tabs、控制與 Snapshot 顯示。
- `adaptive-skill-lifecycle.md` 透過既定 event／Snapshot contract 加入 adaptation 狀態及證據。
- shared contract 與插入點先在總 PRD或 backend contract 定義。
- 兩份子 PRD 不得各自創造不相容的 UI state。

這是責任邊界，不是預先指定程式檔案。

## 建議的 final 子 PRD結構

```text
sub-PRDs/
├── DEPENDENCIES.md
├── scheduling-arena.md
└── adaptive-skill-lifecycle.md
```

`demo-experience.md` 暫不建立。只有日後證明它是完整、可獨立驗收的功能才加入。

五個技術名稱仍可保留於 backend contract、總 PRD 架構及各子 PRD責任段落，不各自成為子 PRD。

## 比賽當日使用 `$to-tickets` 的界線

1. Owner 認領並核對子 PRD與 dependencies。
2. 建立 GitHub parent issue。
3. 要求 `$to-tickets` 草擬 2～3 張 vertical-slice Ticket，最多 5 張。
4. 檢查每張 Ticket 是否可獨立展示或驗證、適合一個新 context window。
5. 檢查 blocking edges 是否是真正阻斷，而非一般整合關係。
6. 由使用者確認粒度、依賴及合併／拆分。
7. 確認 tracker 已設定後，才發布正式 Ticket。
8. 超過 5 張時停止發布，由 main branch 負責人重評子 PRD或拆解方式。

`$to-tickets` 不會僅因被呼叫便保證數量合理。使用者審核、本地上限及 tracker 設定仍不可省略。

## 本輪結論

接受兩份主要子 PRD方案，取代先前五個技術子 PRD候選。它更符合完整功能及 vertical-slice 原則，也較能控制 Ticket 數量。尚須在正式文件中處理 contract 依賴、UI 共用邊界及五張 Ticket 上限，不能只靠 skill 預設。
