# Gradio 跨平台開發研究快取

日期：2026-09-10  
性質：工具技術研究；不是賽事規則、產品需求或正式交接內容。

## 已查證事實

- Gradio 官方安裝指南分別提供 Windows 與 macOS/Linux 的虛擬環境安裝方式。[Gradio：Installing Gradio in a Virtual Environment](https://gradio.app/guides/installing-gradio-in-a-virtual-environment)
- Gradio Quickstart 說明 Python 應用啟動後，會在瀏覽器的本機位址 `localhost:7860` 顯示介面。[Gradio：Quickstart](https://www.gradio.app/main/guides/quickstart)
- `uv` 的專案版面文件說明，`uv.lock` 是跨平台 lockfile（鎖定檔）；解析文件說明其中可含作業系統、硬體架構與 Python 版本的條件標記，安裝時會依所在平台解析相符項目。[uv：Project layout](https://docs.astral.sh/uv/concepts/projects/layout/)；[uv：Resolution](https://docs.astral.sh/uv/concepts/resolution/)

## 推論與限制

依上述工具文件推論，Windows 與 macOS 隊員可共同開發同一個 Gradio 專案，並用同一份 `uv.lock` 對齊依賴版本。

此推論不表示兩個系統的介面與行為已相容。`uv.lock` 只協助依平台解析及安裝依賴，不能證明瀏覽器顯示、字型、縮放、檔案路徑或功能結果一致。

本研究未實際啟動本專案的 UI，也未執行跨平台測試；因此不保證本專案相容。

## 尚未決議的建議

以下是待團隊決定的工作方式，不是新增產品要求：

- 共用 Python 版本與 `uv.lock`，各隊員維護自己的 `.venv`（虛擬環境）。
- Windows 與 macOS 各自啟動一次，檢查 UI 是否可用。
- 選定一台正式展示機，在展示前執行完整流程。

## 待驗證風險

- 瀏覽器差異。
- 字型與顯示縮放。
- 檔案路徑差異。
