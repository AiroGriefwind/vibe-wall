# Vibe Wall - BACKENDSTRUCTURE

## 1. 数据模型

### 1.1 用户（使用 Firebase Auth，不单独建 collection）

- 来源于 Firebase 的 `user` 对象：
  - `uid`
  - `displayName`
  - `photoURL`
  - `email`

### 1.2 Vibe 文档（Firestore collection: `vibes`）

字段示例：

```ts
{
  id: string;          // Firestore 自动 ID
  userId: string;      // Firebase uid
  userName: string;    // displayName 或 email 前缀
  text: string;        // 用户输入内容
  tag: string;         // "摆烂" | "亢奋" | "冷静" | "无标签" 等
  createdAt: Timestamp // Firestore serverTimestamp
}
