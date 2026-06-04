---
name: sh-chemistry-diagram
description: >
  高中化學圖示與結構 SVG 產生器。當任何情境需要生成、繪製或配圖高中化學實驗裝置、
  分子結構、反應位能圖、電化學電池或相圖時，請使用此技能。
  觸發情境包含：「幫我畫滴定實驗裝置圖」、「畫路易斯結構式」、「產生吸熱反應位能圖」、「畫鋅銅電池示意圖」、
  「畫二氧化碳相圖」、「繪製化學分子骨架式」、「化學考卷配圖」等。
  支援圖形類型：實驗裝置（滴定、過濾、蒸餾、排水集氣）、分子與晶體結構（骨架式、路易斯結構、晶胞結構）、
  反應位能圖（吸熱、放熱、催化劑對照）、電化學電池裝置（伽凡尼電池、電解池）、三相圖（水、二氧化碳）。
  圖形可匯出至 Word（.docx）或 PowerPoint（.pptx）。
---

# 高中化學圖示與結構 SVG 產生器技能

## 技能概覽

本技能專門生成高中化學試卷、講義、簡報與教材所需的 SVG 化學示意圖，並輸出為 PNG 圖片檔，可直接插入 Word 文件或 PowerPoint 投影片。

**核心腳本與範本位置**：
```bash
GEOM_DIR=""
for d in /mnt/skills/user/sh-chemistry-diagram/scripts \
          /tmp/sh-chemistry-diagram/scripts; do
  [ -f "$d/chemistry_renderer.py" ] && GEOM_DIR="$d" && break
done
echo "化學繪圖腳本目錄：$GEOM_DIR"
```

---

## 處理流程

### Step 1：理解圖形需求

根據使用者或出題技能傳送的描述，確定要繪製的化學圖示種類：
1. **分子結構**：是路易斯電子點式，還是有機分子的碳鏈骨架式？
2. **實驗裝置**：是酸鹼滴定、溶液過濾、減壓蒸餾，還是氣體收集？
3. **反應位能**：吸熱反應、放熱反應，是否要畫出「有加催化劑」與「未加催化劑」的雙位能曲線對照？
4. **電化學**：鋅銅電池（伽凡尼）還是電解水/電鍍裝置？是否需要標示電子流向與鹽橋？
5. **相圖**：水的相圖（固-液共存線斜率為負）還是二氧化碳相圖（斜率為正）？

### Step 2：建立圖形規格 JSON

依需求建立規格，儲存至 `/home/claude/chemistry_spec.json`。格式如下：

```json
{
  "figures": [
    {
      "id": "fig_titration",
      "type": "experimental_setup",
      "config": {
        "subtype": "titration",
        "liquid_level_ml": 24.5,
        "show_labels": true,
        "buret_liquid_color": "#ffc0cb",
        "flask_liquid_color": "#ffffff"
      },
      "canvas": {"width": 280, "height": 350}
    }
  ],
  "options": {"format": "png", "dpi": 150}
}
```

---

## 支援的化學圖形規格與參數

### 1. molecular_structure（分子與晶體結構）

| 參數 | 說明 | 可選值 |
|------|------|--------|
| `subtype` | 結構樣式 | `lewis`（電子點式）、`skeletal`（有機骨架式）、`crystal_cell`（晶胞結構） |
| `atoms` | 原子列表（lewis/crystal 用） | 點位與元素：`[{"id":"O1","type":"O","x":100,"y":100}]` |
| `bonds` | 化學鍵列表（lewis/crystal 用） | 鍵結：`[{"from":"C1","to":"O1","type":"double"}]` |
| `smiles` | 有機分子 smiles 碼（skeletal 用） | 如 `"CC(O)=O"`（乙酸）、`"c1ccccc1"`（苯） |
| `cell_type` | 晶胞種類（crystal 用） | `fcc`（面心立方）、`bcc`（體心立方）、`nacl`（氯化鈉晶胞） |

**Lewis 結構範例（水分子）**：
```json
{
  "type": "molecular_structure",
  "config": {
    "subtype": "lewis",
    "atoms": [
      {"id": "O1", "type": "O", "x": 100, "y": 100, "lone_pairs": 2},
      {"id": "H1", "type": "H", "x": 50, "y": 70},
      {"id": "H2", "type": "H", "x": 150, "y": 70}
    ],
    "bonds": [
      {"from": "O1", "to": "H1", "type": "single"},
      {"from": "O1", "to": "H2", "type": "single"}
    ]
  }
}
```

---

### 2. experimental_setup（化學實驗裝置）

| 參數 | 說明 | 可選值 |
|------|------|--------|
| `subtype` | 實驗類型 | `titration`（酸鹼滴定）、`filtration`（過濾）、`distillation`（蒸餾）、`gas_collection`（集氣） |
| `show_labels` | 是否標示組件名稱 | `true`（預設）、`false` |
| `liquid_level_ml` | 滴定管液面刻度 | 數值（例如 `12.8`） |
| `buret_liquid_color` | 滴定管液體顏色 | 十六進位顏色碼（如粉紅色表示酚酞變色 `#ff9999`） |
| `distill_thermometer_c` | 蒸餾溫度計示數 | 數值（如 `78.3`） |
| `collection_type` | 集氣方式 | `upward`（向上排氣）、`downward`（向下排氣）、`water`（排水集氣） |

**酸鹼滴定裝置範例**：
```json
{
  "type": "experimental_setup",
  "config": {
    "subtype": "titration",
    "liquid_level_ml": 15.0,
    "buret_liquid_color": "#ff9999",
    "flask_liquid_color": "#ffffff",
    "show_labels": true
  }
}
```

---

### 3. reaction_coordinate（反應位能對照圖）

| 參數 | 說明 | 預設/可選值 |
|------|------|-----------|
| `reaction_type` | 反應類型 | `exothermic`（放熱反應）、`endothermic`（吸熱反應） |
| `has_catalyst` | 是否顯示催化路徑 | `true`（顯示無催化與有催化兩條曲線）、`false` |
| `reactant_energy` | 反應物位能高度 | 數值（如 `100`） |
| `product_energy` | 產物位能高度 | 數值（如 `40` 放熱，`160` 吸熱） |
| `activation_energy` | 活化能峰值高度 | 數值（如 `220`） |
| `catalyst_activation_energy` | 催化後活化能峰值高度 | 數值（如 `150`） |
| `show_energy_values` | 是否標記數值與軸線 | `true`、`false` |

**放熱反應含催化劑對照範例**：
```json
{
  "type": "reaction_coordinate",
  "config": {
    "reaction_type": "exothermic",
    "has_catalyst": true,
    "reactant_energy": 120,
    "product_energy": 50,
    "activation_energy": 250,
    "catalyst_activation_energy": 180,
    "show_energy_values": true
  }
}
```

---

### 4. electrochemical_cell（電化學電池裝置）

| 參數 | 說明 | 可選值 |
|------|------|--------|
| `cell_type` | 電池類型 | `galvanic`（化學電池）、`electrolytic`（電解池） |
| `left_metal` | 左側電極金屬片 | 如 `"Zn"`, `"Cu"`, `"Pt"` |
| `right_metal` | 右側電極金屬片 | 如 `"Cu"`, `"Ag"`, `"Pt"` |
| `left_ion` | 左側電解質主要陽離子 | 如 `"Zn2+"`, `"H+"` |
| `right_ion` | 右側電解質主要陽離子 | 如 `"Cu2+"`, `"Ag+"` |
| `show_salt_bridge` | 是否顯示鹽橋 | `true`（化學電池預設）、`false` |
| `show_electron_flow` | 是否標示外電路電子流向 | `true`（顯示箭頭與 e-）、`false` |

**鋅銅電池（Galvanic Cell）範例**：
```json
{
  "type": "electrochemical_cell",
  "config": {
    "cell_type": "galvanic",
    "left_metal": "Zn",
    "right_metal": "Cu",
    "left_ion": "Zn2+",
    "right_ion": "Cu2+",
    "show_salt_bridge": true,
    "show_electron_flow": true
  }
}
```

---

### 5. phase_diagram（物質三相圖）

| 參數 | 說明 | 可選值 |
|------|------|--------|
| `substance` | 物質種類 | `water`（固液共存線斜率為負，熔點隨壓力增加而降低）、`co2`（斜率為正） |
| `triple_point_p` | 三相點壓力標記 | 數值（如水 `0.006 atm`，二氧化碳 `5.1 atm`） |
| `triple_point_t` | 三相點溫度標記 | 數值（如水 `0.01 °C`，二氧化碳 `-56.6 °C`） |
| `show_regions` | 是否在區域內寫入「固體/液體/氣體」 | `true`（預設）、`false` |

**二氧化碳三相圖範例**：
```json
{
  "type": "phase_diagram",
  "config": {
    "substance": "co2",
    "triple_point_p": "5.1 atm",
    "triple_point_t": "-56.6 °C",
    "show_regions": true
  }
}
```

---

## 輸出與整合

1. **獨立繪圖**：AI 產生規格 JSON 後，呼叫繪圖腳本（利用 matplotlib、cairo 或 RDKit 等底層庫），渲染產出 SVG 格式，隨後轉換為 PNG 圖片格式，儲存在 `/home/claude/geometry_output/` 下。
2. **考卷配圖**：在 `sh-chemistry-exam` 或 `sh-chemistry-context-questions` 出題完畢後，AI 會自動掃描 JSON 題目中的 `geometry` 規格，呼叫本繪圖模組，並在產出題目卷 Word 檔後，執行 python-docx 後處理腳本，將產出的 PNG 圖片無縫插入對應的題幹與子題間。
3. **簡報插圖**：與教學簡報技能配合，可直接將產出的化學圖示貼入簡報投影片中。
