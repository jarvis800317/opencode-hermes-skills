---
name: github-auth
description: GitHub 認證與 MCP 設定。當使用者說「登入 GitHub」、「設定 GitHub MCP」、「連接 GitHub」時，執行 gh auth login 與 GitHub MCP 設定。
---

# GitHub 認證與 MCP 設定

設定 GitHub CLI 登入與 GitHub MCP 整合。

## 觸發時機

- 「登入 GitHub」
- 「設定 GitHub MCP」
- 「連接 GitHub」
- 「檢查 GitHub 狀態」

## 步驟一：檢查 GitHub CLI 登入狀態

```bash
gh auth status
```

確認輸出包含：
- `Logged in to github.com account <帳號>`
- `Active account: true`
- `Token scopes: ... repo ...`

## 步驟二：GitHub 登入（若尚未登入）

```bash
gh auth login --web --git-protocol https
```

流程：
1. 終端機顯示驗證碼
2. 瀏覽器開啟 `https://github.com/login/device`
3. 輸入驗證碼並授權
4. 確認終端機顯示登入成功

## 步驟三：設定 Git 使用者資訊

```bash
git config --global user.name "你的姓名"
git config --global user.email "你的email"
```

## 步驟四：設定 GitHub MCP

在 `~/.config/opencode/opencode.json`（或專案 `opencode.json`）加入：

```json
{
  "mcp": {
    "github": {
      "type": "remote",
      "url": "https://api.githubcopilot.com/mcp/",
      "headers": {
        "Authorization": "Bearer {env:GITHUB_TOKEN}"
      },
      "oauth": false,
      "enabled": true
    }
  }
}
```

## 步驟五：設定環境變數

在 shell 設定檔（如 `~/.zshrc`）加入：

```bash
export GITHUB_TOKEN=$(gh auth token)
```

## 步驟六：驗證

```bash
gh auth status
```

確認後可測試：

```
幫我確認 GitHub MCP 是否已連接，並列出我可以存取的 repo。
```

## 常用 GitHub 操作

| 操作 | 指令 |
|------|------|
| 建立新 repo | `gh repo create <name> --public --source=. --push` |
| 開啟 GitHub Pages | `gh api repos/{owner}/{repo}/pages -X POST -f "source[branch]=main" -f "source[path]=/"` |
| 檢查 repo 狀態 | `gh repo view` |

## 踩坑筆記

| 狀況 | 解法 |
|------|------|
| `gh --version` Access denied | 允許 OpenCode 讀取 GitHub CLI 設定檔 |
| `gh auth status` 未登入 | 執行 `gh auth login --web --git-protocol https` |
| GitHub MCP 無法連接 | 確認 `GITHUB_TOKEN` 環境變數已設定 |
| push 被拒絕 | 檢查 `gh auth status` 與 git remote |

## 不該做的事

- ❌ 把 token 寫進 AGENTS.md 或任何提交到 repo 的檔案
- ❌ 將 `gh auth login --web` 的驗證碼分享給他人