# Mihon & TachiyomiSY 繁體中文翻譯與校對指南 (GEMINI.md)

本專案是一個開源程式翻譯專案，主要負責將來源字串翻譯為符合台灣習慣的繁體中文（正體中文）。為了維持軟體介面的穩定性、品質與語意一致性，所有參與翻譯與自動化校對的代理程式（如 Google Antigravity）與協作者皆須遵守以下規範。

---

## 1. 格式與 Key 完整性規則 (Format & Key Integrity Rules)

為確保程式編譯正常且介面不毀損，必須嚴格遵守以下格式保護規則：

*   **保持 Key 不變**：無論是 JSON、PO、YAML、XML (Moko Resources) 或 TBX 格式，嚴格禁止修改或翻譯 Key 值（如 XML 中的 `<string name="keep_this_key">` 中的 `name` 屬性）。
*   **格式化變數/佔位符保護**：**絕對不能**翻譯、修改或調整格式化變數的位置與格式（例如：`%s`、`%d`、`%1$s`、`%2$d`、`{0}`、`{username}` 等）。
*   **HTML 標籤與跳脫字元**：
    *   保留所有 HTML 標籤（如 `<b>`、`<i>`、`<u>`、`<br/>` ）。
    *   依據檔案格式正確進行跳脫。以 XML 為例：單引號 `'` 必須跳脫為 `\'`；雙引號 `"` 必須跳脫為 `\"`；和號 `&` 必須轉義為 `&amp;`。
*   **1:1 結構對齊**：翻譯檔與原始語言檔（通常為 `base/strings.xml`）必須維持 1:1 的對應關係，切勿合併、拆分或遺漏任何翻譯項目。

---

## 2. 台灣正體中文翻譯風格指引 (Traditional Chinese (Taiwan) Style Guide)

為求使用者體驗自然流暢，請嚴格遵守台灣在地化（zh-TW / zh-rTW）用語標準：

### 2.1 術語對照表 (Strict Terminology Guardrails)
必須優先參考專案內的術語庫：[glossary/zh_Hant.tbx](file:///C:/Users/DIDI/Documents/Git/mihon-zh_Hant/glossary/zh_Hant.tbx)（例如 `Scanlator` 應翻譯為 `掃譯者`，`backup` 應翻譯為 `備份`）。以下為常見詞彙之對照表，請嚴格遵守，禁止使用非台灣習慣的同義詞（如用「優化」代替「最佳化」）：

| 英文原詞 (English) | 台灣繁體中文 (zh-TW) | 禁用詞彙 (Mainland Chinese or others) |
| :--- | :--- | :--- |
| App | 應用程式 | 應用、App |
| optimize / optimization | 最佳化 | 優化 |
| uninstall | 解除安裝 | 卸載 |
| settings / setup / configure | 設定 | 設置 |
| plugin | 外掛 | 插件 |
| background | 背景 | 後台 |
| run / execute | 執行 | 運行 |
| reboot / restart | 重新啟動 | 重啟 |
| lock screen | 鎖定螢幕 | 鎖屏 |
| icon | 圖示 | 圖標 |
| filter | 過濾 / 過濾器 | 篩選 / 濾鏡 |
| user | 使用者 | 用戶 |
| local | 本機 | 本地 |
| firmware | 韌體 | 固件 |
| server address | 伺服器位址 | 服務器地址 |
| debug | 偵錯 | 調試 |
| domain | 網域 | 域名 |
| exclusion | 例外 | 排除 |
| protection | 防護 | 保護 |
| payment | 付款 | 支付 |
| subscriber | 訂閱者 | 訂閱人 |
| network | 網路 | 網絡 |
| extension | 擴充功能 | 擴展 |
| remove / delete | 刪除 | 移除 |
| elements | 元件 | 元素 |
| block / blocked | 封鎖 / 已封鎖 | 屏蔽 / 已屏蔽 |
| port | 連接埠 | 端口 |
| feedback | 意見反應 | 反饋 |
| certificate / CA | 憑證 / 憑證授權單位 | 證書 / 證書授權中心 |
| profiles | 設定檔 | 配置、檔案 |
| code / source code | 程式碼 / 原始碼 | 代碼 / 源代碼 |

### 2.2 句型重構與去翻譯腔 (Syntax & De-translationese)
*   **避免冗餘字眼**：不使用「進行...的動作」、「作為一個...的動作」等冗贅句型。
*   **語意流暢**：中文敘述應符合台灣人的口語習慣，以主動句為主，少用「被」字句（被動式）。
*   **全形標點符號**：中文句子一律使用全形標點符號（，。？！），並使用頓號（、）分隔並列詞彙。

---

## 3. 如何使用 `agy` 進行翻譯校對 (How to use `agy` for Proofreading)

Google Antigravity CLI (`agy`) 是此專案推薦的 AI 翻譯校對工具，能讀取本文件並自動完成檔案比對與詞彙校正。

### 3.1 啟動 `agy`
在專案根目錄下打開終端機，執行：
```bash
agy
```

### 3.2 常用校對指令範例
在 `agy` TUI 介面中，可使用自然語言直接指揮 AI 執行任務：

*   **指定檔案校對**：
    > 「請幫我校對 [zh-rTW/strings.xml](file:///C:/Users/DIDI/Documents/Git/mihon-zh_Hant/mihon/i18n/src/commonMain/moko-resources/zh-rTW/strings.xml)，並比對 [base/strings.xml](file:///C:/Users/DIDI/Documents/Git/mihon-zh_Hant/mihon/i18n/src/commonMain/moko-resources/base/strings.xml) 確認變數格式是否一致。」
*   **全專案翻譯審查**：
    > 「請依據 GEMINI.md 的規格，校對專案中所有的翻譯檔案，找出不符合台灣正體中文或格式損壞的地方並修正。」

### 3.3 輔助校對的 `agy` Slash Commands
*   `/context`：確認當前 AI 載入的語境是否包含目標翻譯檔案及術語庫 [glossary/zh_Hant.tbx](file:///C:/Users/DIDI/Documents/Git/mihon-zh_Hant/glossary/zh_Hant.tbx)。
*   `/diff`：即時預覽所有 AI 修改後的字串差異，確認格式與排版是否完好。
*   `/skills`：確認翻譯相關的技能（如 `android-l10n-zh-tw`、`translator-zh-tw`）已正確啟用。
*   `/rewind`（或 `/undo`）：若對 AI 調整後的譯文不滿意，可用此指令回滾到前一個對話狀態。
