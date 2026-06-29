---
name: skill-installer
description: Skills 查詢與安裝技能。當使用者說「有哪些 skills」、「檢查 skills」、「安裝某個 skill」時，執行本地 skills 查詢與安裝。
---

# Skill 查詢與安裝

查詢和管理 Hermes Agent 可用的 skills。

## 觸發時機

- 「有哪些 skills」
- 「檢查 skills」
- 「已安裝哪些技能」
- 「列出自訂 skills」
- 「安裝某個 skill」

## 查詢本地 Skills

### Hermes Agent Skills

```bash
ls -la ~/.hermes/skills/
```

### 分類查詢

```bash
# workflow 相關
ls ~/.hermes/skills/workflow/

# productivity 相關
ls ~/.hermes/skills/productivity/

# 軟體開發相關
ls ~/.hermes/skills/software-development/
```

### 讀取 Skill 內容

```bash
# 讀取某個 skill 的 SKILL.md
cat ~/.hermes/skills/<category>/<skill-name>/SKILL.md
```

## 使用 skill_manage 管理 Skills

### 查看所有 Skills

使用 `skills_list` 工具查看所有可用 skills：

```
skills_list()
```

### 查看特定 Skill

使用 `skill_view` 查看 skill 內容：

```
skill_view(name="init-project")
```

### 建立新 Skill

使用 `skill_manage(action='create')` 建立新 skill：

```
skill_manage(
  action="create",
  name="my-new-skill",
  category="workflow",
  content="---\nname: my-new-skill\ndescription: 描述...\n---\n\n# 新技能標題\n\n..."
)
```

### 更新現有 Skill

使用 `skill_manage(action='patch')` 更新 skill：

```
skill_manage(
  action="patch",
  name="init-project",
  old_string="舊內容",
  new_string="新內容"
)
```

## 輸出格式

請用以下格式回報：

```markdown
## Skills 檢查完成

### 已安裝 Skills

| Category | Skill | 用途 |
|----------|-------|------|
| workflow | init-project | 初始化新專案 |
| workflow | 收工 | 收工同步 |
| workflow | 繼續 | 繼續工作 |
| ... | ... | ... |

### 可用 Categories

- workflow
- productivity
- software-development
- ...
```

## 安全規則

- ❌ 不安裝來路不明的 skill（先做 Security Scan）
- ❌ 不修改系統預設 skill（除非有明確理由）
- ✅ 確認 skill 來源可信後再安裝