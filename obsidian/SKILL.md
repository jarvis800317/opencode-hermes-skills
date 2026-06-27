---
name: obsidian-secondbrain
description: Obsidian 第二大腦設定。當使用者說「設定第二大腦」、「建立 Obsidian」、「設定 Obsidian vault」時，執行 Obsidian vault 初始化與設定。
---

# Obsidian 第二大腦設定

將 Obsidian vault 設定為適合 Hermes Agent 協作的「第二大腦」。

## 觸發時機

- 「設定第二大腦」
- 「建立 Obsidian」
- 「設定 Obsidian vault」
- 「初始化 Obsidian」

## 預設路徑

- Obsidian vault：`~/我的雲端硬碟/2ndBrain/`
- Hermes Agent Obsidian MCP：已透過 `mcp_obsidian_*` 工具整合

## 步驟一：確認 Vault 路徑

```bash
ls -la ~/我的雲端硬碟/2ndBrain/
```

若 vault 不存在或路徑不同，請告知使用者正確路徑。

## 步驟二：建立三層資料夾結構

```
<VAULT_PATH>/
├── Clippings/           # 輸入：網路文章、外部資料
├── 知識庫/              # 消化：AI 整理後的結構化知識
├── 創作庫/              # 輸出：自己的作品
├── 每日筆記/            # 時間管理：每日紀錄、週計畫
├── Templates/           # 純 Markdown 模板
└── InitialProject/      # 專案工作筆記（Hermes Agent 用）
```

## 步驟三：建立 Vault AGENTS.md

位置：`<VAULT_PATH>/AGENTS.md`

```markdown
# 第二大腦 — AGENTS.md

## 使用者身份

- 身份：<使用者身份>
- 主要用途：<教學 / 研究 / 創作 / 專案管理>
- 語言：繁體中文

## 資料夾規則

| 資料夾 | 用途 | 原則 |
|--------|------|------|
| `Clippings/` | 外部輸入 | 原始資料，不任意改寫 |
| `知識庫/` | 結構化知識 | AI 可協助整理，使用者審核 |
| `創作庫/` | 自己的作品 | 不改寫個人聲音 |
| `每日筆記/` | 時間紀錄 | 記錄每日、每週與重整結果 |
| `Templates/` | 模板 | 純 Markdown，避免外掛依賴 |
| `InitialProject/` | 專案工作筆記 | Hermes Agent 同步用 |

## 新增筆記規則

- 正式筆記要加 frontmatter：title、date、type、tags
- 優先用 Obsidian 雙向連結 [[...]]
- 新增知識庫頁面後，要更新 `知識庫/index.md`
- 重要操作要追加到 `知識庫/log.md`

## 安全規則

- 不覆寫既有筆記；如果檔案存在，先讀原檔再詢問
- 不刪除筆記，除非使用者明確指定
- 不把 API Key、token、密碼寫進筆記
```

## 步驟四：建立模板

### `Templates/每日筆記.md`

```markdown
---
title: 每日筆記
date: YYYY-MM-DD
type: daily-note
tags: [每日筆記]
---

# YYYY-MM-DD 每日筆記

## 今日重點

- （待填）

## 工作紀錄

- （待填）

## 收集到的材料

- （待填）

## 明日優先事項

1. （待填）
```

### `Templates/知識庫頁面.md`

```markdown
---
title: 知識庫頁面
type: knowledge
date: YYYY-MM-DD
tags: []
source:
---

# 知識庫頁面標題

## 核心概念

用一段話說明這篇筆記在解決什麼問題。

## 重點整理

- （待填）

## 與既有筆記的連結

- [[相關筆記]]
```

## 步驟五：建立知識庫初始檔案

### `知識庫/index.md`

```markdown
---
title: 知識庫索引
type: index
updated: YYYY-MM-DD
tags: [知識庫]
---

# 知識庫索引

> 最後更新：YYYY-MM-DD

## 主題總覽

| 主題 | 一行摘要 | 相關筆記 |
|------|----------|----------|
| （等待第一次整理） | | |

## 待補缺口

- （待填）
```

### `知識庫/log.md`

```markdown
---
title: 知識庫操作紀錄
type: log
tags: [知識庫, log]
---

# 知識庫操作紀錄

## YYYY-MM-DD 初始化

**操作類型：** 初始化

**建立內容：** Clippings / 知識庫 / 創作庫 / 每日筆記 / Templates / AGENTS.md

**備註：** 預設不覆寫既有筆記。
```

## 驗證清單

- [ ] Vault 路徑已確認
- [ ] 三層資料夾已建立
- `Clippings/`、`知識庫/`、`創作庫/`、`每日筆記/`、`Templates/`、`InitialProject/`
- [ ] Vault AGENTS.md 已建立
- [ ] 模板已建立
- [ ] `知識庫/index.md` 與 `知識庫/log.md` 已建立

## 與 Hermes Agent 的整合

Hermes Agent 已透過 MCP 整合 Obsidian，可使用：
- `mcp_obsidian_read_note` — 讀取筆記
- `mcp_obsidian_write_note` — 寫入筆記
- `mcp_obsidian_search_notes` — 搜尋
- `mcp_obsidian_patch_note` — 部分更新

## 不該做的事

- ❌ 覆寫既有筆記（使用「補缺，不覆蓋」原則）
- ❌ 刪除筆記（除非使用者明確指定）
- ❌ 將憑證寫入筆記