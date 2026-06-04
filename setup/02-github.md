---
name: antigravity-github
description: 在 AntiGravity 連接 GitHub CLI。說「連接 GitHub」「設定 GitHub」時載入。
---

# 連接 GitHub（AntiGravity 版 - 未安裝）

⚠️ **本機環境尚未偵測到 Git 與 GitHub CLI（gh）**。請先完成安裝後，再執行以下設定：

## 1. 安裝指南
* **Git**：請至 [Git 官網](https://git-scm.com/) 下載並安裝。
* **GitHub CLI**：請至 [GitHub CLI 官網](https://cli.github.com/) 或在 Windows Terminal 執行 `winget install --id GitHub.cli` 下載並安裝。

## 2. 登入步驟
當您安裝完成後，可手動於 PowerShell 執行：
* **檢查登入狀態**：
  ```powershell
  gh auth status
  ```
* **登入 GitHub**：
  ```powershell
  gh auth login --web --git-protocol https
  ```

## 3. 設定全域 Git 帳號資訊
```powershell
git config --global user.name "您的名字"
git config --global user.email "您的信箱或 GitHub no-reply 信箱"
```

## 4. 安全規則
* 不要將 GitHub Token 寫入程式碼或任何 markdown 說明文件中。
* 在 commit 前，務必檢視 `git diff`，避免無差別將敏感資料提交。
