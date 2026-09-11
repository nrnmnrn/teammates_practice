# Session 2 提示詞：唯讀驗收 build-handoff_final

請對已建立的 `build-handoff_final/` 做唯讀、證據導向的獨立驗收。

目的：驗證 final 保留 `build-handoff_2/` 的最新版題目與資訊，並保留正式基準及 `build-handoff/` 的必要流程能力。

## 邊界

- 全程唯讀。
- 不修改或建立任何檔案。
- 不補文件、不建立 Ticket 或 GitHub Issue。
- 不 commit、push、merge 或 deploy。
- 發現問題時，只報告最低修正，不執行修正。

使用 `$caveman:verify-and-stop` 將規則轉為可驗證條件，取得最小充分證據後停止。使用 `$caveman:caveman` 的 `wenyan-ultra` 模式回報。不使用 `$code-review`、`$research` 或 `$task-closeout`。

## 步驟 1：載入權威

完整閱讀：

1. repository 根目錄及適用範圍內的 `AGENTS.md`。
2. `docs/operations/build-handoff-final-sessions/DECISIONS.md`。
3. `docs/competition/README.md`。
4. `docs/operations/team-readiness.md`。
5. `docs/playbook/workflow.md`。
6. `docs/operations/team-work-allocation.md`。
7. `build-handoff/sub-prd-authoring.md`。
8. `DECISIONS.md` 所列主要討論來源中，與驗收爭點有關的完整紀錄。

完成條件：先列出產品、流程、舊 handoff、AO 草案及 final 候選的權威關係；不得以舊產品敘述否決最新版題目。

## 步驟 2：完整盤點，按需精讀

使用 `rg --files` 盤點：

- `build-handoff/`
- `build-handoff_2/`
- `build-handoff_final/`

建立跨目錄內容對照，不限同名檔案。先以路徑、標題、引用、大小、hash 及關鍵詞分類，再完整閱讀所有權威來源、final 文件、疑似衝突文件及可能含獨有需求的文件。

AO-001 至 AO-010 只用於確認是否被錯誤升格或是否揭露總 PRD 的 coverage 缺口；不得把草案結構當成正式要求。

完成條件：三個目錄每個檔案都有對照角色，且所有產品與流程結論均能追溯至內容，不只依目錄結構判斷。

## 步驟 3：獨立雙軸驗收

使用兩位 `gpt-5.6-terra` 唯讀 reviewer，`fork_turns: "none"`：

- Reviewer A 只做產品保真。比較 `build-handoff_2/` 與 final，檢查目標、範圍、功能、非功能要求、Live Run、Sandbox、Scheduler、Agent、Skill lifecycle、UI、Demo、contract、驗收及未決產品決策。
- Reviewer B 只做流程保真。比較兩份正式流程文件、`build-handoff/` 的流程落地方式與 final，檢查文件權威、總 PRD／子 PRD／parent issue／Ticket 層級、Owner、工作順序、依賴、branch、Draft PR、測試、AI review、人類確認、commit、merge 及 closeout。

每位 reviewer：

- 不修改檔案。
- 只回報其軸線的證據表。
- 每項附完整路徑、行號及簡短證據。
- 區分文件事實、證據推論及無法確認。
- 只使用：符合、部分符合、不符合、無證據、尚待決定。
- 不重述長段來源。
- 若指定模型不可用，停止並回報，不得替換模型。

主 Agent 對所有阻斷性發現及抽樣非阻斷發現做原文複核。只有兩份報告出現實質衝突時，才另派一位 fresh `gpt-5.6-terra` 唯讀 reviewer，範圍只限衝突事項。

完成條件：產品與流程分別得出可追溯結論；未以一軸的偏好代替另一軸的權威。

## 步驟 4：必要驗收條件

至少檢查：

- 題目目標、需求範圍、功能及非功能要求。
- `build-handoff_2/` 的資訊有無遺漏、改義或擅自擴張。
- 尚未核准的產品決策是否被當成確定需求。
- AO-001 至 AO-010 是否被搬入或升格為正式 Ticket。
- 總 PRD 是否由子 PRD 完整覆蓋。
- 子 PRD 是否為完整功能切片，而非水平技術工作。
- 每份是否合理預估為 2 至 3 張 vertical-slice Ticket。
- 4 至 5 張的特別核准及超過 5 張的拆分規則。
- `DEPENDENCIES.md` 與各子 PRD 是否一致。
- Contract-ready 平行開發與真正 integration blocker 是否有區分。
- Owner 的認領時機、權限與未指派狀態。
- parent issue、Ticket、branch、Draft PR、acceptance、review、commit、merge 及 closeout。
- 子 PRD 整體驗收與單張 Ticket 完成是否正確區分。
- 文件權威、路徑、連結、術語及 open question 是否一致。
- final 是否能脫離 planning repository 使用。

不得把 `build-handoff/` 的缺檔直接判為 final 違規。先判斷該檔案代表強制流程、舊版實作方式或歷史結構。

## 步驟 5：輸出

使用以下章節：

A. 執行摘要

B. 三方檔案與內容對照表

C. 題目與資訊保真矩陣

D. 流程合規矩陣：規則、狀態、基準證據、final 證據、影響、最低修正

E. 子 PRD coverage、Ticket 粒度與依賴檢查

F. 阻斷性問題

G. 非阻斷改善項

H. 尚待團隊或主辦方回答的問題

I. 最終判定，只能選一項：

- 可直接取代 `build-handoff/`
- 修正後可取代
- 僅可作草稿
- 不應使用

每項判定必須附完整路徑、行號、短證據、事實或推論類型、影響及最低修正。不得只憑目錄結構或個人偏好下結論。

依 repository Completion Report 補充執行命令、假設、限制、風險與完整 ELI5。明確聲明未修改任何檔案。證據已足以作出上述判定時立即停止。
