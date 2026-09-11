# 子 PRD 數量、完整性與 Ticket 規模討論紀錄

- 日期：2026-09-12
- 性質：對話紀錄，供後續討論追蹤
- 權威限制：本文件不是總 PRD、子 PRD、Ticket、核准紀錄或流程規範。若與正式文件衝突，以正式文件及最新團隊決議為準。

## 本輪疑問

于喬詢問：

- 為何整個題目可以只有兩至三份子 PRD？
- 子 PRD 是否太少？
- 完成這些子 PRD 是否等於整題完成？
- 每份子 PRD 再拆成 Ticket 是否仍會過大？

## 核心判斷

子 PRD 數量不是完整性的證明。兩份可能足夠，五份也可能漏需求。判定須同時通過：

1. 總 PRD每項必要能力都有且只有清楚的負責子 PRD。
2. 每份子 PRD是一項完整、可獨立驗收與合併的功能。
3. 每份通常可拆成 2～3 張 vertical-slice Ticket，最多 5 張。
4. 每張 Ticket 可由一個全新 context window 完成，並有獨立可見或可驗證結果。
5. 全部子 PRD完成後，仍須通過跨功能整合、Demo、測試、review 與 main 驗證。

因此，「所有子 PRD完成」是整題完成的必要條件，不是單獨的充分條件。

## 兩份方案的覆蓋能力

原兩份方案在功能概念上可覆蓋多數需求：

- `scheduling-arena.md`：workload、Scheduler、policies、job state、events、metrics、基礎 UI 與控制。
- `adaptive-skill-lifecycle.md`：trigger、既有 Skill 重用、Candidate、Sandbox、Evaluator、Critic、Skill Library、自動啟用及相關 UI。

若每項總 PRD要求都明確映射至這兩份，理論上兩份即可覆蓋產品。但第二份同時包含兩條不同成功路徑：

1. 找到可用的既有 Skill，直接選擇並啟用。
2. 所有既有 Skills 失敗，生成 Candidate，迭代、驗證、註冊並啟用。

兩條路徑各有獨立條件、錯誤、驗收及 UI 證據。合為一份雖具同一 adaptation 主題，卻可能超過五張合理 Ticket。

## 修正後較穩健的三份方案

### `scheduling-arena.md`

交付基本可運作、可觀察的排程 Arena：

- workload 輸入及控制。
- Scheduler 及基礎 policies。
- job lifecycle、Snapshot、events 與 metrics。
- 基礎 Gradio UI。
- 播放、暫停、單步、重設及瀏覽器驗收。

預估 2～3 張 vertical-slice Ticket。

### `existing-skill-adaptation.md`

交付第一條 adaptation 成功路徑：

- 從 metrics 偵測退化。
- 以相同條件評估既有 Skills。
- 選出最佳合格 Skill。
- 下一次 dispatch 自動啟用。
- UI 顯示原因、比較結果及 activation event。
- 找到既有 Skill 時不得建立 Candidate。

預估 2～3 張 vertical-slice Ticket。

### `candidate-skill-lifecycle.md`

交付第二條 adaptation 成功與失敗路徑：

- 只在全部既有 Skills 失敗後生成 Candidate。
- Sandbox 與 Evaluator gate。
- Critic feedback 與最多五版迭代。
- 通過後註冊 Skill，於下一次 dispatch 啟用。
- 五版失敗或 adapter 錯誤時保留最後有效狀態。
- UI 顯示 Candidate、評估、feedback、註冊、啟用或失敗證據。

預估 3～4 張 vertical-slice Ticket。

此三分法不是按 Scheduler、Sandbox、UI 等技術層切割。每份都由觸發一路走到可見結果，仍屬完整垂直功能。

## 為何三份不算太少

子 PRD是完整功能容器，不是工作清單。其數量取決於產品的獨立驗收路徑，而非程式模組數量。

三份可涵蓋：

- 基礎排程。
- 重用既有 Skill 的調適。
- 生成新 Skill 的調適。

UI、Sandbox、Skill Library、Adapter 與測試分配到這三條完整路徑，不需各自成為水平子 PRD。

## 完成三份是否等於整題完成

尚不等於。還須通過總體 closeout：

- 所有總 PRD需求均有證據。
- 三份子 PRD在最新 main 一起運作。
- 共用 contract 無衝突。
- 正常 workload、Flash Sale、既有 Skill 重用及 Candidate 路徑皆通過。
- 失敗及 fallback 行為符合規格。
- 三個 Gradio tabs、動畫、metrics、events 與 Skill Library 使用同一 Snapshot。
- 90 秒正式 Demo 路徑可完成。
- 完整測試、最終 AI review、Owner 自我檢查、另一位成員確認及 main branch 負責人整合檢查完成。

因此，正確關係是：三份子 PRD完成，加上總體驗收與整合完成，才可宣告整題完成。

## Ticket 是否仍太大

目前只能估算，不能證明。實作 repo、可重用程式與現場條件尚未知。

判定 Ticket 過大的警訊：

- 一張同時引入多個新外部 adapter。
- 一張需要大量後端、UI、狀態機及瀏覽器工作，無法形成窄路徑。
- 驗收只能等後續 Ticket完成。
- 一個全新 context window 難以完成。
- 為遵守五張上限而把多項行為塞入同一張。

若三份方案仍需每份超過五張，不應把 Ticket 壓大。應選擇：

- 檢查是否有真正獨立的第四份子 PRD。
- 縮減產品範圍並由團隊正式決定。
- 利用已存在的程式或 adapter 降低工作量。

拆出更多子 PRD不會增加實際工作；只會讓原本隱藏在大文件中的工作量可見。

## `DEPENDENCIES.md` 的建議關係

```text
scheduling-arena
├── existing-skill-adaptation
└── candidate-skill-lifecycle

candidate-skill-lifecycle 另依賴 existing-skill-adaptation 的
trigger、existing-skill evaluation result 與「全部失敗」判定。
```

實際阻斷仍須區分完整子 PRD依賴與 contract 先行即可平行的依賴。

## 本輪結論

- 兩份子 PRD是可討論的最小候選，不是已證明的最終答案。
- 依總 PRD兩條 adaptation 成功路徑，三份較穩健。
- 暫薦 `scheduling-arena.md`、`existing-skill-adaptation.md`、`candidate-skill-lifecycle.md`。
- 三份全完成後仍須總體整合與驗收，才算整題完成。
- Ticket 規模須於比賽當日依實作 repo 驗證；不得用五張上限掩蓋過大 Ticket。
