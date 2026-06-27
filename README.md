# OpenCode/Hermes Agent Skills 完整套件

> 將 01-09 設定資料夾整理為 Hermes Agent 可用的 skills 格式
>
> 🐙 GitHub：https://github.com/jarvis800317/opencode-hermes-skills

## 目錄結構

```
opencode-hermes-skills/
├── setup/                    # 01-EnvironmentSetting：環境建置
├── github/                   # 02-GitHub：GitHub 連接與 MCP
├── notebooklm/               # 03-NotebookLM：Google NotebookLM 連接
├── obsidian/                 # 04-Obsidian：第二大腦設定
├── firebase/                 # 05-Firebase：Firebase Firestore 整合
├── mcp/                      # 06-MCP：瀏覽器控制與桌面自動化
├── workflow/                 # 07-InitialProject：專案工作流程
└── references/               # 參考文檔與踩坑紀錄
```

## 快速開始

1. 依序執行每個 skill 的設定
2. 每個 skill 獨立的，可根據需求選擇性安裝
3. 建議安裝順序：
   - `setup`（必備）
   - `github`（強烈建議）
   - `notebooklm`、`obsidian`、`firebase`、`mcp`（按需選擇）
   - `workflow`（專案管理工作流）

## 各模組說明

| Skill | 用途 | 必裝 |
|-------|------|------|
| `setup` | 安裝 Git、Node.js、uv、登入 GitHub | ✅ |
| `github` | 設定 GitHub MCP、commit/push/GitHub Pages | 建議 |
| `notebooklm` | NotebookLM AI 生成簡報/音頻/影片 | 選配 |
| `obsidian` | 第二大腦知識管理 | 選配 |
| `firebase` | Firestore 資料庫 CRUD | 選配 |
| `mcp` | 瀏覽器控制、桌面自動化 | 選配 |
| `workflow` | init-project / 收工 / 繼續 工作流 | 建議 |

## 安全規則

- ❌ 不將 API Key、token、憑證寫入任何檔案
- ❌ 不 commit `.env`、`.codex/`、`node_modules/`
- ✅ 安裝前先做 Security Scan
- ✅ 對外訊息先展示草稿