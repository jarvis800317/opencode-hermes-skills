---
name: setup
description: 環境建置 skill。當使用者說「環境建置」、「檢查環境」、「安裝基礎工具」時，執行基礎工具檢查與安裝（Git、GitHub CLI、Node.js、uv）。
---

# 環境建置

檢查並安裝 Hermes Agent / OpenCode 所需的基礎工具。

## 觸發時機

- 「環境建置」
- 「檢查環境」
- 「安裝基礎工具」
- 「設定開發環境」

## 工具檢查清單

執行並記錄結果：

```bash
git --version
gh --version
node --version
uv --version
```

## 安裝指引

### Git

**macOS：**
```bash
xcode-select --install
```

**Windows：**
```bash
winget install --id Git.Git --accept-source-agreements --accept-package-agreements
```

**Linux：**
```bash
sudo apt update && sudo apt install git -y
```

### GitHub CLI

**macOS：**
```bash
brew install gh
```

**Windows：**
```bash
winget install --id GitHub.cli --accept-source-agreements --accept-package-agreements
```

**Linux：** 见 [GitHub CLI 官方安裝指引](https://github.com/cli/cli/blob/trunk/docs/install_linux.md)

### Node.js（版本需 18+）

**macOS：**
```bash
brew install node
```

**Windows：**
```bash
winget install --id OpenJS.NodeJS --accept-source-agreements --accept-package-agreements
```

**Linux：**
```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
```

### uv（Python 工具管理器）

**macOS / Linux：**
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**Windows（PowerShell）：**
```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

## 環境檢查表格

| 工具 | 預期版本 | 實際版本 | 狀態 |
|------|----------|----------|------|
| Git | 任意穩定版 |  |  |
| GitHub CLI（gh）| 任意穩定版 |  |  |
| Node.js | 18+ |  |  |
| uv | 任意版本 |  |  |

## 輸出報告

完成後請用以下格式回報：

```
## 環境建置完成

### 工具狀態

| 工具 | 版本 | 狀態 |
|------|------|------|
| Git | x.x.x | ✅ 已安裝 / ❌ 未安裝 |
| GitHub CLI | x.x.x | ✅ 已安裝 / ❌ 未安裝 |
| Node.js | x.x.x | ✅ 18+ / ❌ 低於 18 / ❌ 未安裝 |
| uv | x.x.x | ✅ 已安裝 / ❌ 未安裝 |

### 下一步

- [ ] 設定 Git 使用者資訊（若尚未設定）
- [ ] 登入 GitHub（若尚未登入）
- [ ] 繼續設定所需的 MCP 或 skills
```