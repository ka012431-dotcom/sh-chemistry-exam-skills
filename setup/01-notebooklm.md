---
name: antigravity-notebooklm
description: 在 AntiGravity 連接 NotebookLM MCP。說「連接 NotebookLM」「設定 NotebookLM」時載入。
---

# 連接 NotebookLM（AntiGravity 版 - 已設定）

本機已成功於 Python 3.11 環境安裝並完成 Google 帳號授權驗證。

## 本機執行路徑
* 程式路徑：`C:\Users\ka012\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\LocalCache\local-packages\Python311\Scripts\nlm.exe`

## 常見指令
* **重新登入**：
  ```powershell
  & "C:\Users\ka012\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\LocalCache\local-packages\Python311\Scripts\nlm.exe" login
  ```
* **診斷狀態**：
  ```powershell
  & "C:\Users\ka012\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\LocalCache\local-packages\Python311\Scripts\nlm.exe" doctor
  ```
* **列出筆記本**：
  ```powershell
  & "C:\Users\ka012\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\LocalCache\local-packages\Python311\Scripts\nlm.exe" list
  ```

## 註冊 MCP 範本
在 AntiGravity 的 MCP 設定檔（例如 `mcp-config.json` 或設定介面中）加入以下設定：
```json
"notebooklm": {
  "type": "local",
  "command": [
    "C:\\Users\\ka012\\AppData\\Local\\Packages\\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\\LocalCache\\local-packages\\Python311\\Scripts\\nlm.exe",
    "mcp"
  ],
  "enabled": true
}
```

⚠️ 安全守則：
1. 不要複製 cookie/token。
2. 不將筆記本清單或 `notebooks.json` 提交（commit）到 GitHub 儲存庫。
