---
name: antigravity-firebase
description: 在 AntiGravity 連接 Firebase MCP。說「連接 Firebase」「設定 Firebase」時載入。
---

# 連接 Firebase（AntiGravity 版 - 未安裝）

⚠️ **本機環境尚未偵測到 Node.js / NPM**。Firebase CLI 工具需要 Node.js 環境才能執行。

## 1. 安裝指南
* **Node.js**：請前往 [Node.js 官網](https://nodejs.org/) 下載安裝 LTS 版本（會自動包含 NPM）。
* 安裝後，重啟終端機並驗證：
  ```powershell
  node -v
  npm -v
  ```

## 2. 登入 Firebase
安裝完成後，於 PowerShell 執行：
* **登入 Firebase 帳戶**：
  ```powershell
  npx.cmd -y firebase-tools@latest login
  ```
* **列出所有專案進行驗證**：
  ```powershell
  npx.cmd -y firebase-tools@latest projects:list
  ```

## 3. 註冊 Firebase MCP 範本
在 AntiGravity 的 MCP 設定檔中註冊以下伺服器：
```json
"firebase": {
  "type": "local",
  "command": ["npx.cmd", "-y", "firebase-tools@latest", "mcp"],
  "enabled": true
}
```

⚠️ 安全規則：
* Firebase 前端 config 可以公開，但 **Admin SDK 服務帳戶私鑰 (.json)** 絕對不能 commit 到儲存庫！
* 請確保 `.firebaserc` 中無敏感專案 ID 洩漏。
