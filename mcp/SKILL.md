---
name: browser-automation
description: 瀏覽器控制與桌面自動化 MCP 設定。當使用者說「設定瀏覽器控制」、「安裝 Playwright MCP」、「設定桌面自動化」時，執行 MCP 安裝與設定。
---

# 瀏覽器控制與桌面自動化 MCP

整合 Playwright MCP（瀏覽器控制）與 open-computer-use（桌面自動化）。

## 觸發時機

- 「設定瀏覽器控制」
- 「安裝 Playwright MCP」
- 「設定桌面自動化」
- 「瀏覽器自動化」

## 步驟一：安裝 Playwright MCP

```bash
npm install -g @playwright/mcp
npx playwright install chromium
```

Playwright MCP 提供 23 個瀏覽器自動化工具。

## 步驟二：安裝 open-computer-use

```bash
npm install -g open-computer-use
```

open-computer-use 提供 9 個桌面自動化工具。

## 步驟三：設定 MCP

在 `opencode.json` 加入：

```json
{
  "mcp": {
    "playwright": {
      "type": "local",
      "command": ["npx", "-y", "@playwright/mcp"],
      "enabled": true
    },
    "open-computer-use": {
      "type": "local",
      "command": ["open-computer-use", "mcp"],
      "enabled": true
    }
  }
}
```

## 步驟四：重啟並驗證

重啟 Hermes Agent 後可測試：
- 「開啟瀏覽器到 example.com」
- 「截圖目前頁面」
- 「列出目前執行中的程式」

## 踩坑筆記

| 狀況 | 解法 |
|------|------|
| open-computer-use 不能用 npx 啟動 | 必須用 `open-computer-use mcp` |
| Windows install-opencode-mcp 失敗 | 手動編輯 opencode.json |
| Playwright 首次需下載瀏覽器 | 先執行 `npx playwright install chromium` |
| notifications/initialized 錯誤 | 可忽略，不影響功能 |

## 工具說明

### Playwright MCP（23 個工具）

- `browser_navigate` — 導航到 URL
- `browser_snapshot` — 取得頁面快照
- `browser_click` — 點擊元素
- `browser_type` — 輸入文字
- `browser_vision` — 視覺分析
- 等等...

### open-computer-use（9 個工具）

- `list_apps` — 列出執行中程式
- `get_app_state` — 取得視窗狀態
- `mouse_click` — 滑鼠點擊
- `keyboard_type` — 鍵盤輸入
- 等等...