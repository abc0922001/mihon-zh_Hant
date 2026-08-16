# Mihon & 分支專案繁體中文（台灣）在地化與翻譯指南 (GEMINI.md)

本專案 (`mihon-zh_Hant`) 為開源漫畫閱讀器 **Mihon** 以及其衍生分支版本（**TachiyomiSY**、**TachiyomiJ2K**）的繁體中文（台灣，`zh-TW` / `zh-rTW` / `zh_Hant`）在地化專案。

本指南定義了專案結構、檔案規格、術語字典、格式跳脫規則以及 AI 協作校對標準，供所有貢獻者與 AI 代理程式（如 Google Antigravity / Gemini）遵循。

---

## 1. 專案架構與目錄規範

專案針對不同上游軟體採用各自對應的在地化資源架構：

```text
mihon-zh_Hant/
├── glossary/
│   └── zh_Hant.tbx                     # TBX (ISO 12200) 格式之術語對照庫
├── mihon/                              # Mihon 主專案 (基於 Moko Resources)
│   └── i18n/src/commonMain/moko-resources/
│       ├── base/                       # 英文來源 (strings.xml, plurals.xml)
│       └── zh-rTW/                     # 台灣繁體中文 (strings.xml, plurals.xml)
├── tachiyomisy/                        # TachiyomiSY 分支 (基於 Moko Resources)
│   └── i18n-sy/src/commonMain/moko-resources/
│       ├── base/                       # 英文來源 (strings.xml, plurals.xml)
│       └── zh-rTW/                     # 台灣繁體中文 (strings.xml, plurals.xml)
├── tachiyomij2k/                       # TachiyomiJ2K 分支 (Android 原生 Resource)
│   └── app/src/main/res/
│       ├── values/                     # 英文來源 (strings.xml)
│       └── values-zh-rTW/              # 台灣繁體中文 (strings.xml)
└── GEMINI.md                           # 本專案知識庫與 AI 協作規範
```

### 1.1 資源架構特點
*   **Moko Resources (`mihon`, `tachiyomisy`)**：
    *   路徑為 `src/commonMain/moko-resources/<locale>/`。
    *   基準語言位於 `base/`，繁體中文位於 `zh-rTW/`。
    *   支援 `strings.xml` 與 `plurals.xml`。
*   **Android Native Resources (`tachiyomij2k`)**：
    *   路徑為 `app/src/main/res/values<qualifier>/`。
    *   基準語言位於 `values/`，繁體中文位於 `values-zh-rTW/`。
*   **術語庫 (`glossary/zh_Hant.tbx`)**：
    *   採用標準 TBX (TermBase eXchange) XML 格式，記錄專有名詞（如 `Scanlator` 譯為 `掃譯者`）。

---

## 2. 格式完整性與跳脫規則 (Format & Integrity Guardrails)

為避免編譯失敗、應用程式閃退或介面版面錯亂，必須嚴格落實以下技術限制：

### 2.1 Key 值與標籤保護
*   **禁止修改 Key 名稱**：`<string name="...">` 或 `<plurals name="...">` 中的 `name` 屬性必須與來源檔完全一致，絕不能翻譯或修改。
*   **保留非翻譯屬性**：若標籤包含 `translatable="false"` 或格式化相關屬性，需依照來源檔規範保留。

### 2.2 格式化佔位符保護 (Format Specifiers)
*   **絕對禁止翻譯或變更佔位符結構**：包含 `%s`、`%d`、`%1$s`、`%2$d`、`%1$d`、`%.2f`、`{0}` 等。
*   **保留順序標記**：當句子中包含多個變數時，確保數字引號（如 `%1$s`、`%2$s`）對應正確的來源語意。

### 2.3 XML 特殊字元與跳脫規則 (Escaping)
*   **單引號 (`'`)**：必須跳脫為 `\'`（例如：`Don\'t`）。
*   **雙引號 (`"`)**：必須跳脫為 `\"`，或使用全形引號「」『』。
*   **和號 (`&`)**：必須轉義為 `&amp;`。
*   **小於 / 大於符號 (`<` / `>`)**：除 HTML 標籤外，需轉義為 `&lt;` 與 `&gt;`。
*   **換行符號 (`\n`)**：保留換行控制碼 `\n`。
*   **HTML 樣式標籤**：保留 `<b>`、`</b>`、`<i>`、`</i>`、`<u>`、`</u>`、`<tt>`、`</tt>` 等標籤，勿破壞閉合結構。

### 2.4 複數規則 (`plurals.xml`)
*   英文來源檔常包含 `quantity="one"` 與 `quantity="other"`。
*   在繁體中文 (`zh-rTW`) 語法中，通常只需保留 `<item quantity="other">` 項目，除非特定語境需要對特定數量做個別處理。

---

## 3. 台灣繁體中文 (zh-TW) 在地化風格與術語表

翻譯必須符合台灣本土用語習慣（Seamless Localization），杜絕非台灣習慣詞彙與機器翻譯腔。

### 3.1 核心術語對照表 (Strict Terminology Guardrails)

| 英文術語 (English) | 台灣繁體中文 (zh-TW) | 禁用詞彙 / 簡中習慣 (Forbidden) |
| :--- | :--- | :--- |
| **App** | 應用程式 | 應用、App |
| **Optimize / Optimization** | 最佳化 | 優化 |
| **Uninstall** | 解除安裝 | 卸載 |
| **Settings / Setup / Configure** | 設定 | 設置、配置 |
| **Plugin** | 外掛 | 插件 |
| **Extension** | 擴充功能 | 擴展、插件 |
| **Background** | 背景 | 後台 |
| **Run / Execute** | 執行 | 運行 |
| **Reboot / Restart** | 重新啟動 | 重啟 |
| **Lock Screen** | 鎖定螢幕 | 鎖屏 |
| **Icon** | 圖示 | 圖標 |
| **Filter / Filtering** | 過濾 / 過濾器 | 篩選 / 濾鏡 |
| **User** | 使用者 | 用戶 |
| **Local** | 本機 | 本地 |
| **Firmware** | 韌體 | 固件 |
| **Server Address** | 伺服器位址 | 服務器地址 |
| **Debug** | 偵錯 | 調試 |
| **Domain** | 網域 | 域名 |
| **Exclusion** | 例外 | 排除 |
| **Protection** | 防護 | 保護 |
| **Payment** | 付款 | 支付 |
| **Subscriber(s)** | 訂閱者 | 訂閱人 |
| **Network** | 網路 | 網絡 |
| **Remove / Delete** | 刪除 | 移除 (依情境)、抹除 |
| **Elements** | 元件 | 元素 |
| **Block / Blocked** | 封鎖 / 已封鎖 | 屏蔽 / 已屏蔽 |
| **Port** | 連接埠 | 端口 |
| **Feedback** | 意見反應 | 反饋 |
| **Certificate / CA** | 憑證 / 憑證授權單位 | 證書 / 證書授權中心 |
| **Profiles** | 設定檔 | 配置、檔案 |
| **Code / Source Code** | 程式碼 / 原始碼 | 代碼 / 源代碼 |
| **Scanlator** | 掃譯者 | 漢化組、掃描者 |
| **Library** | 圖庫 / 書庫 | 書架、資源庫 |
| **Backup** | 備份 | 後備 |
| **Restore** | 還原 | 恢復 |
| **Track / Tracker** | 追蹤 / 追蹤器 (平台) | 記錄、同步器 |
| **Categories** | 類別 | 分類 |
| **Migrate / Migration** | 移轉 | 遷移 |
| **Cache** | 快取 | 緩存 |
| **Default** | 預設 | 默認 |
| **Interface** | 介面 | 界面 |
| **Display** | 顯示 | 屏幕、展現 |
| **Resolution** | 解析度 | 分辨率 |

### 3.2 語法與去翻譯腔 (De-translationese)
1.  **句型重組**：打破過長的從屬子句，依據中文「先因後果」、「先背景後重點」的敘事習慣重組。
2.  **避免冗贅動詞**：刪除「進行...的動作」、「作為一個...」等冗詞。
3.  **主動語態為主**：減少「被」字句（被動語態），改用自然主動句。
4.  **全形標點符號**：
    *   中文語境一律使用全形標點（`，`、`。`、`！`、`？`、`：`、`；`）。
    *   列舉並列名詞時使用頓號（`、`）。
    *   英文專有名詞、版本號與數字前後保留適當空格以增進閱讀體驗。

---

## 4. AI 自動化校對與維護工作流程

使用 Google Antigravity CLI (`agy`) 或 IDE 進行日常翻譯維護時，建議的工作流程如下：

### 4.1 校對檢查清單 (Validation Checklist)
1.  **缺失 Key 補齊**：比對 `base/strings.xml` (或 `values/strings.xml`) 與 `zh-rTW` 檔案，確保無遺漏條目。
2.  **佔位符核對**：確認所有 `%s`、`%d`、`%1$s` 數量與編號完全吻合。
3.  **跳脫符號檢查**：確認英文單引號 `'` 均已跳脫為 `\'`，XML 特殊字元（`&` 等）已正確轉義。
4.  **用語一致性**：對齊本文件之術語表與 `glossary/zh_Hant.tbx`。

### 4.2 常用指令與操作
*   **比對與翻譯**：
    > 「請檢查 [tachiyomij2k/app/src/main/res/values-zh-rTW/strings.xml](file:///C:/Users/DIDI/Documents/Git/mihon-zh_Hant/tachiyomij2k/app/src/main/res/values-zh-rTW/strings.xml) 是否缺少 [values/strings.xml](file:///C:/Users/DIDI/Documents/Git/mihon-zh_Hant/tachiyomij2k/app/src/main/res/values/strings.xml) 中的 Key，並依據 GEMINI.md 翻譯缺失字串。」
*   **術語庫更新**：
    若建立新的專用名詞譯法，同步將詞條補入 [glossary/zh_Hant.tbx](file:///C:/Users/DIDI/Documents/Git/mihon-zh_Hant/glossary/zh_Hant.tbx)。
