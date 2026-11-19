# 政黨與選舉制度 LLM Wiki — AGENTS.md 規範手冊

本檔案是本知識庫的「憲法」與維護規範，定義 Wiki 的架構、本體論、Markdown 格式、雙向鏈結規則、LLM 維護工作流程以及**教學簡報（HTML Slides）製作標準**。

---

## 1. 知識庫架構（Three-Layer Architecture）

依循 Andrej Karpathy 的 LLM Wiki 設計模式，本知識庫分為三層：

1. **原始來源層（Raw Sources）**：`raw/`
   - 存放由國立臺灣大學政治學系王業立教授授課、張建邦摘要之 12 份經典教材。
   - **特性**：唯讀（Immutable）、不可修改，為全庫唯一的「真實之源（Source of Truth）」。
2. **編譯持久層（Compiled Wiki Layer）**：`wiki/`
   - 由 LLM 依據原始文獻閱讀、萃取、交叉參照後持久化編譯的 Markdown 知識庫。
   - 分為概念、制度、政黨、臺灣法制、比較專題。
   - 包含全局索引（`index.md`）、全景導航（`overview.md`）與維護日誌（`log.md`）。
3. **規範層（The Schema / Guidelines）**：`AGENTS.md`
   - 規範 LLM 如何處理新文獻、維護 cross-references、消除衝突、執行 Lint 檢查以及依指示產製簡報。

---

## 2. 目錄與本體論（Ontology）

```text
political_systems_wiki/
├── AGENTS.md                       # 本規範檔案 (AI Agent 核心指引)
├── raw/                            # 12 份原始講義 MD (唯讀 Source of Truth)
├── slides/                         # 【簡報專區】依指示產出之獨立 HTML 教學簡報檔
└── wiki/                           # 編譯後的知識庫持久層
    ├── index.md                    # 依分類條列之完整內容索引
    ├── log.md                      # Append-only 操作與編譯歷史
    ├── overview.md                 # 理論體系架構全景地圖
    ├── concepts/                   # 核心政治學理論、定理、悖論與量化公式
    ├── systems/                    # 各國選舉制度分類、運作機制與代表性
    ├── party_politics/             # 政黨體系、政府組成聯盟理論與黨內提名機制
    ├── taiwan_institutions/        # 臺灣憲政改革、選制變遷、政黨法與國會運作
    └── comparisons/                # 跨制度、跨公式與跨政黨的橫向對比專案
```

---

## 3. 頁面規範與 Frontmatter 範例

每個 Wiki 條目必須具備一致的 YAML Frontmatter，以便 Obsidian 的 Dataview 插件與 Graph View 解析：

```yaml
---
title: 條目正式中文名稱
aliases: [英文簡稱, 別名]
category: concepts | systems | party_politics | taiwan_institutions | comparisons
tags: [政治學, 選舉制度, 理論]
sources: ["[[raw/6_講義_選舉制度的政治影響 (一).md]]"]
updated: 2026-11-19
---
```

### 內文撰寫準則
1. **雙向鏈結（Wikilinks）**：所有重要概念、學者（如杜瓦傑、薩托利、阿羅、孔多塞）、制度名稱與專有名詞，首次出現時必須使用 `[[條目名稱]]` 鏈結。
2. **數學公式**：涉及量化指標（如有效政黨數 $N_v$ / $N_s$、加拉格爾指數 $LSq$、選舉配額公式）必須使用標準 LaTeX 區塊語法，並附帶變數定義與計算範例。
3. **理論與經驗對照**：闡述制度理論時，必須對照教材中提及的具體國家經驗（臺灣、英、美、德、日、法、紐西蘭等）。
4. **溯源引註**：每頁結尾必須設立 `## 參考來源`，清楚指向 `raw/` 內的對應講義章節與頁碼。

---

## 4. LLM 維護作業流程（Operations）

- **Ingest（攝取）**：讀取 raw 來源，萃取核心實體，分散更新至對應資料夾條目，並回寫 `index.md` 與 `log.md`。
- **Query（查詢）**：LLM 優先查閱 `index.md` 與 `overview.md` 鎖定相關頁面，綜合條目回答問題；若產出深具價值之綜合論述，可回存至 `comparisons/`。
- **Lint（健檢）**：檢查是否存在孤島頁面（Orphan pages）、失效鏈結（Broken links）、定義矛盾或遺漏之核心概念。

---

## 5. 簡報製作規則（Presentation Creation Rules）

> ⚠️ **執行原則**：平常維護知識庫時**不主動產生簡報**。僅在使用者**明確提出需求**（如「請將某主題做成 HTML 簡報」）時，才啟動本規範於 `slides/` 生成獨立 HTML 檔案。

### 一、 核心規格與技術框架
1. **技術格式**：全面採用**單一獨立 HTML 簡報檔（Standalone HTML Slides）**。
   - 任何現代瀏覽器（Chrome, Edge, Safari）點擊即可直接全螢幕流暢播放，完全不依賴任何本機外掛或 Markdown 插件。
   - 內建 Reveal.js 與 KaTeX（支援鍵盤左右方向鍵換頁、全螢幕播放 `F` 鍵、幻燈片縮圖概覽 `ESC` 鍵與高解析度數學渲染）。
2. **檔案歸檔與命名**：
   - 所有簡報檔案一律儲存於專案根目錄的 `slides/` 資料夾。
   - 命名格式統一為：`slides/<主題名稱>_簡報.html`。
3. **資訊密度與風格**：
   - 定位為**「學術/教學授課型」**：內容必須嚴謹扎實，包含清晰的名詞定義、數理公式（LaTeX）、制度運作機制、跨國對照表格與學術論證脈絡。
4. **講者備忘稿**：
   - **不需產出講者備忘稿（No Speaker Notes）**，所有核心知識直接精準呈現在投影片版面卡片內。

---

### 二、 視覺設計與美學系統（日式卡哇伊・粉綠彩）

簡報必須具備溫潤、優雅、精緻的日式粉綠色系，嚴禁死板的純黑白工程風格。

#### 1. 指定色票（Palette）
- **主要強調粉（灰粉紅 / Rose Taupe）**：`#C09A97`（用於大標題、主要徽章、強調卡片邊框）
- **輔助薄荷綠（青瓷綠 / Celadon Mint）**：`#94A69D`（用於次標題、次要卡片、進度指示條）
- **溫潤青灰（Dark Slate Gray）**：`#4A504D`（用於正文文字，確保可讀性與親和力）
- **柔和奶白底（Warm Cream / Milk White）**：`#FAF7F2`（投影片主背景色）
- **純白卡片底（Pure Card White）**：`#FFFFFF`（內文卡片區塊）
- **柔和奶黃（Warm Custard）**：`#EFDBB2`（高亮提示框背景）
- **杏奶暖米（Apricot Cream）**：`#E8D0BB`（分組標籤、表格表頭背景）

#### 2. 字體體系（Typography）
採用圓潤、親和且清晰的日系圓體與無襯線字體堆疊：
```css
font-family: 'Zen Maru Gothic', 'Yu Gothic UI', 'Hiragino Maru Gothic ProN', 'Microsoft JhengHei', sans-serif;
```

#### 3. 版面組件（UI Components）
- **圓角卡片風格**：投影片內觀點與公式包裹於圓角卡片中（`border-radius: 18px`），帶有粉綠手繪柔和陰影：
  `box-shadow: 0 8px 24px rgba(148, 166, 157, 0.15);`
- **分欄佈局（Two-column Layout）**：複雜比較或公式演算採用 Flexbox 雙欄卡片並列。
- **進度指示條**：底部的簡報進度條同步呈現薄荷青瓷綠（`#94A69D`）。

---

### 三、 標準 HTML 簡報骨幹結構（Template）

產出 HTML 簡報時，需遵循以下獨立封裝骨架：

```html
<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="utf-8">
  <title>主題名稱 — 教學簡報</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/reveal.js@4.5.0/dist/reveal.css">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/reveal.js@4.5.0/plugin/highlight/monokai.css">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.8/dist/katex.min.css">
  <style>
    :root {
      --bg-cream: #FAF7F2;
      --card-white: #FFFFFF;
      --pink-primary: #C09A97;
      --mint-green: #94A69D;
      --text-dark: #4A504D;
      --border-beige: #E8D0BB;
    }
    body, .reveal {
      background-color: var(--bg-cream);
      font-family: 'Zen Maru Gothic', 'Yu Gothic UI', 'Hiragino Maru Gothic ProN', 'Microsoft JhengHei', sans-serif;
      color: var(--text-dark);
    }
    .reveal h1 {
      color: var(--pink-primary);
      font-size: 2.2em;
      border-bottom: 3px dashed var(--mint-green);
      padding-bottom: 12px;
    }
    .reveal h2 { color: var(--mint-green); font-size: 1.6em; }
    .card {
      background: var(--card-white);
      border: 2px solid var(--border-beige);
      border-radius: 18px;
      padding: 24px;
      box-shadow: 0 8px 24px rgba(148, 166, 157, 0.15);
      text-align: left;
      margin: 12px 0;
    }
    .badge {
      display: inline-block;
      background-color: var(--mint-green);
      color: white;
      padding: 4px 14px;
      border-radius: 20px;
      font-size: 0.7em;
      font-weight: bold;
    }
    .badge-pink { background-color: var(--pink-primary); }
    .columns { display: flex; gap: 20px; }
    .col { flex: 1; }
  </style>
</head>
<body>
  <div class="reveal">
    <div class="slides">
      <!-- 投影片內容 sections -->
    </div>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/reveal.js@4.5.0/dist/reveal.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/reveal.js@4.5.0/plugin/math/math.js"></script>
  <script>
    Reveal.initialize({
      hash: true,
      plugins: [ RevealMath.KaTeX ]
    });
  </script>
</body>
</html>
```
