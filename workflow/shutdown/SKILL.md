---
name: 收工
description: 收工同步技能。當使用者說「收工」、「結束了」、「準備換電腦」、「該同步的同步」、「先到這裡」時，執行收工同步：更新 Obsidian 工作筆記、git commit + push。依「專案初始化開工與收工通案標準」執行追加模式。
---

# 收工同步技能

對話結束前，把今天的工作完整保存。依據「專案初始化開工與收工通案標準」執行。

## 固定檔案

每個專案必備：
- `AGENTS.md`
- `<專案資料夾名稱>工作筆記.md`
- `.gitignore`

Obsidian 路徑：
```
~/我的雲端硬碟/2ndBrain/InitialProject/<專案資料夾名稱>工作筆記.md
```

## 觸發語句

- 「收工」
- 「結束了」
- 「準備換電腦」
- 「該同步的同步」
- 「先到這裡」

## 收工 SOP

### 步驟 1：盤點今天做了什麼

從對話歷史中分析：
- 完成了哪些檔案？
- 做了哪些決策？
- 踩到哪些新坑？

**只有本次有實質檔案異動或耐久決策時才執行。**

若對話中沒有實質變動（只是問問題），則回應：「今天沒有實質進度，略過同步。」結束技能。

### 步驟 2：確認專案路徑

從對話脈絡推斷當前專案：
- 本機：`~/我的雲端硬碟/Jarvis專案/projects/<專案名>/`
- Obsidian：`~/我的雲端硬碟/2ndBrain/InitialProject/<專案名>工作筆記.md`

若無法確認，詢問使用者：「請告訴我專案名稱。」

### 步驟 3：讀取現有工作筆記

讀取 `<專案名>工作筆記.md`，確認現有內容，不覆蓋歷史。

### 步驟 4：追加工作筆記

在工作筆記中追加：
- 更新「上次做到哪」
- 在「最近更動紀錄」表格新增一行（**不覆蓋歷史**）
- 若有新踩坑，在「踩坑筆記」追加

**寫入模式：追加，不覆蓋歷史。**

### 步驟 5：同步 Obsidian

將更新後的工作筆記同步到 Obsidian 對應路徑。

驗證：
```bash
diff ~/我的雲端硬碟/Jarvis專案/projects/{專案名}/{專案名}工作筆記.md ~/我的雲端硬碟/2ndBrain/InitialProject/{專案名}工作筆記.md
```

### 步驟 6：Git commit + push

```bash
cd ~/我的雲端硬碟/Jarvis專案/projects/{專案名}
git status -sb
git add -A
git commit -m "{標題行}

- {具體變動1}
- {具體變動2}
- {踩坑記錄（若有）}"
git push origin main
```

**不 commit 的檔案**：
- `.codex/`、`.claude/`（代理設定，含敏感資訊）
- `node_modules/`
- `*.log`
- `.env`（除非要 commit）
- `*~`、`*.tmp`
- 私人來源文件及正式輸出文件

**不 commit 的來源敏感資訊**：
- API Key、token、credentials
- 資料庫連線字串
- 私人文件

若專案尚非 git repo，先初始化：
```bash
cd ~/我的雲端硬碟/Jarvis專案/projects/{專案名}
git init
gh repo create {專案名} --private --source=. --push
```

### 步驟 7：驗證 GitHub

```bash
gh repo view jarvis800317/{專案名} --json name,url,pushedAt
```

確認 commit 已推送到 GitHub。

## 報告同步狀態

提供五勾表格：

| 平台 | 變動內容 | 狀態 |
|------|----------|------|
| GDrive | 自動同步 | ✅ |
| Obsidian | 工作筆記追加（不覆蓋）| ✅ |
| GitHub | commit + push | ✅ |
| 工具總清單 | hermes-project-workflow/工具清單.md（如有變動）| ✅ |
| Firebase | Firebase 部署（若有 firebase.json）| ⚠️ 跳過 |

## 不該做的事

- ❌ 對「沒實質進度」的對話執行同步（例：純問問題、查資料）
- ❌ commit `.codex/`、`.claude/`、`node_modules/`、`.env`
- ❌ commit message 寫「更新」、「修改」等無意義標題
- ❌ 刪除既有工作日誌內容，應以「追加」為主
- ❌ 覆蓋工作筆記，應保留歷史並追加新內容