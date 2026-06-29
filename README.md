# OpenCode/Hermes Agent Skills 完整套件

> 將 01-09 設定資料夾整理為 Hermes Agent 可用的 skills 格式
>
> 🐙 GitHub：https://github.com/jarvis800317/opencode-hermes-skills

## 目錄結構

```
opencode-hermes-skills/
├── setup/                    # 01-EnvironmentSetting：環境建置
│   ├── SKILL.md              # Git、Node.js、uv 檢查
│   └── skill-installer/      # Skills 查詢與安裝
├── github/                   # 02-GitHub：GitHub 連接與 MCP
│   └── github-auth/          # gh login + GitHub MCP
├── notebooklm/               # 03-NotebookLM：Google NotebookLM 連接
│   └── SKILL.md
├── obsidian/                 # 04-Obsidian：第二大腦設定
│   └── SKILL.md
├── firebase/                 # 05-Firebase：Firebase Firestore 整合
│   └── SKILL.md
├── mcp/                      # 06-MCP：瀏覽器控制與桌面自動化
│   └── SKILL.md
├── productivity/             # 實用工具
│   ├── document-processing/  # PDF/Word/Excel 處理
│   └── media-processing/     # 影音下載與處理
├── workflow/                 # 07-InitialProject：專案工作流程
│   ├── init-project/         # 初始化新專案
│   ├── shutdown/             # 收工同步（收工）
│   ├── resume/               # 繼續工作（繼續）
│   └── project-lifecycle/   # 專案生命週期整合
└── references/               # 參考文檔
    ├── 專案初始化開工與收工通案標準.md
    ├── 踩坑紀錄.md
    └── 本機已安裝工具清冊.md
```

## Skills 列表

| Category | Skill | 用途 |
|----------|-------|------|
| setup | `setup` | 環境建置（Git、Node.js、uv） |
| setup | `skill-installer` | Skills 查詢與安裝 |
| github | `github-auth` | GitHub 登入與 MCP 設定 |
| notebooklm | `notebooklm` | NotebookLM AI 生成工具 |
| obsidian | `obsidian-secondbrain` | Obsidian 第二大腦設定 |
| firebase | `firebase-firestore` | Firebase Firestore 整合 |
| mcp | `browser-automation` | 瀏覽器控制與桌面自動化 |
| productivity | `document-processing` | PDF/Word/Excel 處理 |
| productivity | `media-processing` | 影音下載與處理 |
| workflow | `init-project` | 初始化新專案 |
| workflow | `收工` | 收工同步 |
| workflow | `繼續` | 繼續工作 |
| workflow | `project-lifecycle` | 專案生命週期整合 |

## 快速開始

### 1. 安裝依賴工具

```bash
# Git（macOS）
xcode-select --install

# GitHub CLI
brew install gh  # macOS
winget install --id GitHub.cli  # Windows

# Node.js
brew install node  # macOS

# uv（Python 工具管理器）
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### 2. 檢查環境

```
「檢查環境」
```

### 3. 登入 GitHub

```
「登入 GitHub」
```

## Python 套件（可选）

```bash
# PDF 處理
pip3 install pypdf PyMuPDF pdfplumber pdf2image reportlab

# 影音處理
pip3 install yt-dlp youtube-transcript-api

# Word/Excel 處理
pip3 install python-docx openpyxl
```

## 安全規則

- ❌ 不將 API Key、token、憑證寫入任何檔案
- ❌ 不 commit `.env`、`node_modules/`、`.codex/`、`*.log`
- ✅ 安裝前先做 Security Scan
- ✅ 對外訊息先展示草稿

## 更新日誌

| 日期 | 更新內容 |
|------|----------|
| 2026-06-29 | 新增 `skill-installer`、`document-processing`、`media-processing`、`project-lifecycle`，更新本機工具清冊 |
| 2026-06-27 | 初始版本：環境建置、GitHub、NotebookLM、Obsidian、Firebase、MCP、Workflow skills |