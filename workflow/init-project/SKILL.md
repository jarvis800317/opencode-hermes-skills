---
name: init-project
description: 初始化新專案。當使用者說「開新專案」、「新建專案」、「我想做一個 XXX 工具」時，幫助建立專案資料夾、初始化 Git、建立 Obsidian 工作筆記。
---

# 初始化新專案

快速建立一個新專案的基礎結構。依據「專案初始化開工與收工通案標準」執行。

## 觸發時機

使用者說：
- 「初始化這個專案」
- 「建立專案工作模式」
- 「套用InitialProject模板」
- 「開新專案」
- 「新建專案」
- 「我想做一個 XXX 工具」

## 固定檔案

每個專案根目錄必備：
- `AGENTS.md`：長期穩定規則與初始化／開工／收工 SOP
- `<專案資料夾名稱>工作筆記.md`：持續追加的進度、決策、驗證、待辦及踩坑
- `.gitignore`：排除憑證、代理設定、快取、暫存、來源機密文件及正式輸出

Obsidian 同步路徑：
```
~/我的雲端硬碟/2ndBrain/InitialProject/<專案資料夾名稱>工作筆記.md
```

本機與 Obsidian 工作筆記必須同名且內容一致。

## SOP

### 步驟 1：詢問專案名稱

若使用者只說「開新專案」，詢問：「專案叫什麼名字？」

專案命名規則：
- 英文小寫 + 連字號（例：`coordinate-hunter`、`taolin-tool`）
- 不需要 .md 或 .html 副檔名

### 步驟 2：建立專案結構

```bash
# 建立專案資料夾
mkdir -p ~/我的雲端硬碟/Jarvis專案/projects/{專案名}

# 在 Obsidian 建立專案資料夾
mkdir -p ~/我的雲端硬碟/2ndBrain/InitialProject
```

### 步驟 3：初始化獨立 Git + GitHub

```bash
cd ~/我的雲端硬碟/Jarvis專案/projects/{專案名}
git init
gh repo create {專案名} --private --source=. --push
```

> 預設使用 private，若使用者指定 public 則用 `--public`

### 步驟 4：建立 AGENTS.md

在專案資料夾建立 `AGENTS.md`（內容見下方範本）

### 步驟 5：建立工作筆記

在 `~/我的雲端硬碟/2ndBrain/InitialProject/{專案名}工作筆記.md` 建立

### 步驟 6：建立 .gitignore

```bash
# 敏感資料
.env
*.key
credentials.*

# 系統檔
.DS_Store
*.tmp
~$*

# 代理與快取
.codex/
.claude/
.proxy*

# Node.js（若需要）
node_modules/
package-lock.json
```

### 步驟 7：同步並驗證

```bash
diff ~/我的雲端硬碟/Jarvis專案/projects/{專案名}/{專案名}工作筆記.md ~/我的雲端硬碟/2ndBrain/InitialProject/{專案名}工作筆記.md
```

### 步驟 8：報告完成

## AGENTS.md 範本

```markdown
# {專案名}

## 專案目的

（描述這個工具做什麼）

## 工作狀態

- 進度：0%（剛初始化）
- 開始日期：{今天}
- GitHub：https://github.com/jarvis800317/{專案名}

## 工作節奏

| 觸發語句 | 執行動作 |
|---------|---------|
| 「開工」 | 讀取工作筆記、檢查 Git 狀態、輸出摘要 |
| 「收工」 | 盤點、寫入工作筆記、同步 Obsidian、commit + push |
| 「繼續」 | 讀取工作進度繼續工作 |

## 安全規則

- 安裝任何外部套件、skill、MCP 前，先做 Security Scan
- 對外訊息先出草稿，不直接送出
- 不可把 API Key、token、憑證檔提交到版本庫

## 收工 SOP

1. 盤點本次工作
2. 追加本機工作筆記，不覆蓋歷史
3. 同步 Obsidian 同名筆記並驗證
4. 確認 Git repo、remote、branch
5. Stage 本次預定發布的來源檔
6. Commit、push 並回報三方狀態
```

## 工作筆記範本

```markdown
# {專案名} 工作筆記

> 📌 持續追加的進度、決策、驗證、待辦及踩坑。每次收工時更新，不覆蓋歷史。

## 上次做到哪

**最後動作**：初始化專案
**完成檔案**：AGENTS.md、.gitignore
**對話脈絡**：剛建立專案，還沒有實質進度

## 最近更動紀錄

| 日期 | 變更摘要 | GDrive | Obsidian | GitHub |
|------|----------|--------|----------|--------|
| {今天} | 初始化專案 | ✅ | ✅ | ✅ |

## 待辦事項

（待填寫）

## 踩坑筆記

（待記錄）
```

## 不該做的事

- ❌ 在未確認的情況下覆蓋既有專案
- ❌ commit `.codex/`、`.claude/`、`node_modules/`、`.env`
- ❌ 初始化完成後未建立工作筆記