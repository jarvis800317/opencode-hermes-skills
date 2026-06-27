---
name: notebooklm
description: NotebookLM MCP 設定。當使用者說「設定 NotebookLM」、「連接 NotebookLM」、「安裝 nlm」時，執行 NotebookLM MCP 安裝與設定。
---

# NotebookLM MCP 設定

將 Google NotebookLM 整合到 Hermes Agent / OpenCode。

## 觸發時機

- 「設定 NotebookLM」
- 「連接 NotebookLM」
- 「安裝 nlm」
- 「NotebookLM 懶人包」

## 原理

```
Hermes Agent ←(MCP)→ notebooklm-mcp（MCP server）←(Google 登入)→ NotebookLM
```

## 步驟一：檢查環境

```bash
git --version
uv --version
```

若 uv 未安裝，先執行 setup skill。

## 步驟二：安裝 NotebookLM MCP CLI

```bash
uv tool install notebooklm-mcp-cli
```

確認：
```bash
nlm --version
```

若 `nlm: command not found`，嘗試：
```bash
# 重設路徑
export PATH="$(uv tool dir --bin):$PATH"
nlm --version
```

## 步驟三：登入 Google 帳號

```bash
nlm login
```

瀏覽器會開啟 Google 登入頁，登入後 CLI 自動擷取認證。

確認：
```bash
nlm doctor
```

## 步驟四：設定 MCP

### 方法 A：用 nlm 一鍵設定（推薦）

```bash
nlm setup add opencode
```

### 方法 B：手動編輯 opencode.json

在 `~/.config/opencode/opencode.json` 加入：

```json
{
  "mcp": {
    "notebooklm": {
      "type": "local",
      "command": ["notebooklm-mcp"],
      "enabled": true,
      "timeout": 300000
    }
  },
  "experimental": {
    "mcp_timeout": 300000
  }
}
```

### 方法 C：使用完整路徑（若方法 A/B 失敗）

```json
{
  "mcp": {
    "notebooklm": {
      "type": "local",
      "command": ["C:\\Users\\<你>\\AppData\\Roaming\\uv\\tools\\notebooklm-mcp-cli\\Scripts\\notebooklm-mcp.exe"],
      "enabled": true,
      "timeout": 300000
    }
  }
}
```

Windows 路徑需用雙反斜線 `\\\\`。

## 步驟五：重啟並驗證

設定檔變更後需**完全重啟** Hermes Agent。

驗證指令：
```
列出我的 NotebookLM 筆記本清單
```

成功後可測試：
- 「建立一個叫『測試』的 notebook」
- 「產生簡報」
- 「產生音訊」

## 功能說明

NotebookLM MCP 可生成：
- Slides（簡報）
- Infographics（資訊圖表）
- Audio（音訊）
- Video（影片）
- Docs（文件）
- Sheets（試算表）
- Mindmaps（心智圖）
- Quizzes（測驗）

## 踩坑筆記

| 狀況 | 解法 |
|------|------|
| `nlm: command not found` | 重開終端機；將 `uv tool dir --bin` 加入 PATH |
| 登入後 `nlm doctor` 未認證 | 重跑 `nlm login` |
| 設定改了沒生效 | 必須重啟 Hermes Agent |
| Windows 路徑格式錯誤 | 使用 `\\\\` 或直接寫 `/` |

## 復原方式

```bash
# 移除 MCP 設定
# 編輯 opencode.json 刪除 notebooklm 段

# 移除 nlm
uv tool uninstall notebooklm-mcp-cli
nlm logout 2>/dev/null
```