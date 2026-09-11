# 子 PRD Ticket 上限與 Candidate lifecycle 拆分討論紀錄

- 日期：2026-09-12
- 性質：對話紀錄，供後續討論追蹤
- 權威限制：本文件不是總 PRD、子 PRD、Ticket、核准紀錄或流程規範。若與正式文件衝突，以正式文件及最新團隊決議為準。

## 本輪由于喬提出的方向

- 每份子 PRD希望維持 2～3 張 Ticket。
- 若預估超過 3 張，傾向再拆子 PRD。
- 目的在限制失敗範圍，避免一份子 PRD出問題時，過多工作同時被卡住。
- 若此規則確定，後續須修改 `build-handoff/sub-prd-authoring.md`，並同步反映至 `build-handoff_final/`。
- 詢問是否應再拆 `candidate-skill-lifecycle.md`。
- 本輪先討論及記錄，不修改 handoff。

## 對 2～3 張 Ticket 規則的判斷

控制每份子 PRD規模的方向正確。但不建議把 3 張設為完全無例外的硬上限。

Ticket 數量只是規模訊號，不是產品邊界本身。若一項完整功能自然需要 4 張獨立 vertical-slice Ticket，為符合數字強拆，可能造成：

- 新子 PRD無法獨立交付或驗收。
- 增加 GitHub parent issue、branch、Draft PR 與 closeout 成本。
- 增加跨子 PRD依賴及等待。
- 原本一個完整功能被切成前半、後半，前半完成仍無使用者價值。

較穩健規則：

- 標準：2～3 張 Ticket。
- 4～5 張：例外；須說明為何仍是一項不可再自然拆分的完整功能，並由 main branch 負責人核可。
- 超過 5 張：必須停止，重新評估子 PRD邊界或正式縮減範圍。

如此兼顧小批次與完整功能，不讓 Ticket 數字反向主導產品設計。

## 阻斷風險的真正判定

一份子 PRD有幾張 Ticket，不直接決定會卡住多少工作。真正影響阻斷範圍的是：

- Blocking edges 是否過多。
- 是否把一般整合依賴誤寫成硬阻斷。
- 子 PRD是否共用未穩定 contract。
- 多份子 PRD是否必須修改同一狀態或同一 UI seam。
- 前一份子 PRD是否必須完整 merge，後一份才能開始。

因此，除了控制 Ticket 數量，`DEPENDENCIES.md` 還須標示：硬阻斷、contract 依賴及整合依賴。

## Candidate lifecycle 是否宜再拆

建議拆。理由不是它章節多，而是它包含一道真實的安全與狀態邊界：

1. Candidate 在 sandbox 內生成、迭代及評估，不得修改 Live Run。
2. 只有通過 Evaluator gate 的 Candidate，才可進入 Skill Library並影響下一次 dispatch。

此邊界可形成兩項完整、可觀察且可獨立驗收的功能。

### `candidate-evaluation-loop.md`

交付：

- 只在所有既有 Skills 失敗後建立 Candidate。
- Provider-neutral Planner 產生 Candidate。
- Sandbox 隔離執行。
- Evaluator 產生有理由的通過／拒絕結果。
- Critic feedback 驅動最多五版迭代。
- 錯誤或五版失敗時產生可見證據，不修改 Live Run。

預估 2～3 張 vertical-slice Ticket。

### `skill-promotion-and-recovery.md`

交付：

- 只接受已通過 Evaluator gate 的 Candidate。
- 註冊為 Skill並顯示於 Skill Library。
- 下一次 dispatch 自動啟用。
- 保留正在執行的 job，不搶占。
- 建立 activation event 與 policy segment。
- 註冊、啟用或 adapter 失敗時保留最後有效狀態。
- Reset 恢復初始 Skills。

預估 2～3 張 vertical-slice Ticket。

前一份可獨立證明「新策略在隔離環境中安全地被評估」；後一份可獨立證明「只有合格策略能進入正式執行」。這不是按技術層水平切分，而是按安全閘門前後的產品能力切分。

## 修正後候選子 PRD集合

```text
sub-PRDs/
├── DEPENDENCIES.md
├── scheduling-arena.md
├── existing-skill-adaptation.md
├── candidate-evaluation-loop.md
└── skill-promotion-and-recovery.md
```

預估：

- `scheduling-arena.md`：2～3 張 Ticket。
- `existing-skill-adaptation.md`：2～3 張 Ticket。
- `candidate-evaluation-loop.md`：2～3 張 Ticket。
- `skill-promotion-and-recovery.md`：2～3 張 Ticket。

總數可能為 8～12 張。拆分不會減少實際工作，只會降低每份子 PRD與每次失敗的影響範圍。若總量不適合比賽時間，應縮減範圍或重用既有成果，不能只靠減少子 PRD數量隱藏工作量。

## 依賴建議

```text
scheduling-arena
├── existing-skill-adaptation
│   └── candidate-evaluation-loop
│       └── skill-promotion-and-recovery
└── 共同 integration／Demo 驗收
```

此圖只表示產品邏輯順序。實作不必完全串行：只要 Snapshot、metrics、events、EvaluationResult 與 activation contract 穩定，後續子 PRD可用 deterministic adapter 平行準備。正式 blocking edges 待實作 repo 與 Owner 確認後決定。

## 未來對 `sub-prd-authoring.md` 的建議修改

若团队確認本輪規則，應把現行「通常 2～3、最多 5」改明確為：

- 2～3 張是標準範圍。
- 預估 4～5 張時，作者必須先重新檢查是否存在獨立、可驗收的子功能；若仍不拆，須記錄理由並由 main branch 負責人核可。
- 超過 5 張不可核可，必須重拆或正式縮減範圍。
- 不得只為符合 Ticket 數量，把不能獨立驗收的前後半段拆成不同子 PRD。

同步更新 final 的模板、workflow、INDEXER 與相关說明，避免只有一處寫新規則。

## 本輪結論

- 支持以 2～3 張 Ticket 作每份子 PRD的標準規模。
- 不建議把 3 張設成完全無例外硬上限。
- 4～5 張須重新檢查、記錄理由及核可；超過 5 張必拆。
- 建議把 `candidate-skill-lifecycle.md` 拆成 `candidate-evaluation-loop.md` 與 `skill-promotion-and-recovery.md`。
- 此拆分有明確 sandbox／Live Run 安全邊界，能降低錯誤影響範圍。
- 本輪不修改 `build-handoff/sub-prd-authoring.md`；待于喬正式決定後再執行。
