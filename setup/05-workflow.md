---
name: antigravity-workflow
description: AntiGravity 開工/收工/新專案初始化流程。說「開工」「收工」「初始化專案」時載入。
---

# 開工 / 收工 / 新專案初始化工作流

## 🌅 開工工作流 (Start of Work)
當您對 AI 說「開工」或「開始工作」時，AI 助理應執行：
1. **讀取設定**：自動讀取專案根目錄的 `ANTIGRAVITY.md`。
2. **同步狀態**：檢查 Git 狀態（如可用）與最新 Commit。
3. **整理回報**：向您報告當前開發狀態，並建議下一步的工作計畫。
4. **安全原則**：**不**自動執行 Pull/Fetch 或 Push。

## 🌌 收工工作流 (End of Work)
當您對 AI 說「收工」或「結束工作」時，AI 助理應執行：
1. **敏感資料掃描**：自動掃描並檢查是否有 API Keys、Tokens、憑證、或個人隱私真名被誤寫入代碼中。
2. **規則維護**：僅在專案工作規則或路徑發生改變時，才更新 `ANTIGRAVITY.md`。
3. **變更盤點**：檢視 Git Status 與 `git diff`，引導您只 Stage 相關檔案（避免無差別的 `git add .`）。
4. **提交與同步**：在您確認 Commit Message 後進行 Commit 與 Push（如可用）。

## 🆕 新專案初始化工作流 (New Project Init)
當您對 AI 說「新專案初始化」時：
1. **收集專案資訊**：AI 將先詢問您的專案名稱、用途、工作資料夾、是否建立 GitHub Repo、Obsidian Vault 關聯等。
2. **建立基礎設施**：
   - 建立並補齊 `ANTIGRAVITY.md`。
   - 建立 `README.md`。
   - 建立 `.gitignore` 檔案。
3. **保護既有成果**：若是既有資料夾，AI 將以「只補缺口、絕不覆盖原有設定」為原則執行。
