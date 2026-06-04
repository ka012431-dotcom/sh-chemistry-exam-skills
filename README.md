# 高中化學科教學與命題技能集 (sh-chemistry-exam-skills)

這是一組專為**高中化學科（對齊 108 課綱）**設計的 **Agent Skills**。內含 4 個獨立的 AI 技能，專注於高中化學的「段考命題」、「生活情境與探究實作非選題設計」、「化學圖示/結構/相圖生成」以及「形成性評量互動小遊戲製作」。

本技能集採用**繁體中文**編寫，題目設計對齊修訂版 Bloom 認知層次，適用於支援 Agent Skills 規格的 AI 助理（如 AntiGravity、Claude Code 等）。

---

## 📦 包含的技能

| 技能目錄 | 用途 | 適用情境與觸發詞 |
|---------|------|----------------|
| [`sh-chemistry-exam`](skills/sh-chemistry-exam/SKILL.md) | **高中化學段考命題與審題專家** | 規劃段考雙向細目表、自動產出單選與多選題、進行標準答案分佈檢查、審查現有考卷品質。觸發詞如：「幫我出化學考題」、「審一下這份化學考卷」、「做化學雙向細目表」。 |
| [`sh-chemistry-context-questions`](skills/sh-chemistry-context-questions/SKILL.md) | **高中化學生活情境與探究實作非選擇題** | 結合最新時事、綠色化學、能源科技等真實生活情境，設計包含(1)情境解讀(2)化學原理應用的二小題式非選擇題。觸發詞如：「幫我出化學非選題」、「設計化學探究與實作非選」、「結合生活情境出化學題」。 |
| [`sh-chemistry-diagram`](skills/sh-chemistry-diagram/SKILL.md) | **化學圖示與結構 SVG 產生器** | 生成化學實驗裝置圖（滴定、蒸餾、過濾）、分子結構式（路易斯結構、有機骨架式）、反應位能圖、電化學電池裝置與物質三相圖。觸發詞如：「幫我畫滴定實驗裝置圖」、「畫路易斯結構式」、「產生吸熱反應位能圖」、「畫鋅銅電池示意圖」。 |
| [`chemistry-minigames`](skills/chemistry-minigames/SKILL.md) | **化學評量互動小遊戲產生器** | 自動分析教材重點，並為每個重點產出一個網頁互動小遊戲（包含記憶翻牌、反應式係數平衡填充、俗名用途配對、實驗步驟排序、是非題等），發佈至 GitHub Pages。觸發詞如：「根據化學講義出小遊戲」、「化學教材轉互動網頁」、「製作平衡化學反應式小遊戲」。 |

---

## 🚀 使用指南

### 1. 複製本儲存庫
```bash
git clone https://github.com/ka012431-dotcom/sh-chemistry-exam-skills.git
```

### 2. 安裝到您的 AI 助理技能目錄
將 `skills/` 底下的技能資料夾複製到您的 AI 助理載入路徑中：

*   **AntiGravity (Windows PowerShell)**:
    ```powershell
    Copy-Item sh-chemistry-exam-skills/skills/* C:\Users\您的使用者名稱\.gemini\antigravity\skills\ -Recurse
    ```
*   **Claude Code (Unix)**:
    ```bash
    cp -r sh-chemistry-exam-skills/skills/* ~/.claude/skills/
    ```

---

## 📄 授權條款
本專案採用 [MIT License](LICENSE) 授權。歡迎自由修改、分享與使用於教學現場。
