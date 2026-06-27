---
name: firebase-firestore
description: Firebase Firestore 設定。當使用者說「設定 Firebase」、「連接 Firestore」、「部署 Firestore 規則」時，執行 Firebase 專案設定與 MCP 整合。
---

# Firebase Firestore 設定

將 Firebase Cloud Firestore 整合到 Hermes Agent。

## 觸發時機

- 「設定 Firebase」
- 「連接 Firestore」
- 「部署 Firestore 規則」
- 「Firebase 懶人包」

## 環境需求

- Node.js 18+
- Firebase CLI（透過 npx）
- Firebase 專案（需有 Firestore Database）

## 步驟一：檢查環境

```bash
node --version
npx --version
```

Node.js 需 18 以上。

## 步驟二：確認/建立 Firestore Database

1. 前往 [Firebase Console](https://console.firebase.google.com/)
2. 選擇或建立專案
3. 建立 Firestore Database（選擇「生產模式」或「測試模式」）
4. 選擇資料庫位置（建議 `asia-east1` 或 `asia-northeast1`）

## 步驟三：設定 Firebase MCP

### 方式 A：透過 npx（推薦）

在 `opencode.json` 加入：

```json
{
  "mcp": {
    "firebase": {
      "type": "local",
      "command": ["npx.cmd", "-y", "firebase-tools@latest", "mcp"],
      "enabled": true,
      "timeout": 120000
    }
  }
}
```

> Windows 環境必須用 `npx.cmd` 而非 `npx`

### 方式 B：使用 Firebase MCP Server（官方）

```json
{
  "mcp": {
    "firebase": {
      "type": "remote",
      "url": "https://firebaseai.googleapis.com/mcp",
      "enabled": true
    }
  }
}
```

## 步驟四：部署 Firestore 規則

建立 `firestore.rules`：

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if false;
    }
  }
}
```

部署：
```bash
npx.cmd -y firebase-tools@latest deploy --only firestore:rules
```

## Firestore CRUD 操作

透過 Firebase MCP 可用以下工具：

| 操作 | MCP 工具 | 說明 |
|------|----------|------|
| 新增文件 | `firestore_add_document` | 在指定集合新增文件 |
| 讀取文件 | `firestore_get_document` | 依路徑讀取 |
| 列出文件 | `firestore_list_documents` | 列出集合內文件 |
| 查詢 | `firestore_query_collection` | 條件查詢 |
| 刪除 | `firestore_delete_document` | 刪除文件 |
| 更新 | `firestore_update_document` | 更新欄位 |

## 踩坑筆記

| 狀況 | 解法 |
|------|------|
| Windows 上 npx 權限問題 | 使用 `npx.cmd` |
| MCP 設定變更後不生效 | 重啟 Hermes Agent |
| `read_time cannot be in the future` | 驗證時優先用 add/get/delete |
| EPERM 權限錯誤 | 以系統管理員身分執行 |

## 安全規則

- ❌ 不要將 Firebase Admin SDK 憑證提交到公開 repo
- ❌ 不要將真實資料庫設為 `allow read, write: if true`
- ✅ 規則變更前先測試
- ✅ 定期審查 Firestore 規則

## 參考資源

- [Firebase Console](https://console.firebase.google.com/)
- [Firebase MCP Server 文件](https://firebase.google.com/docs/ai-assistance/mcp-server)