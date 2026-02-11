# Vibe Wall - APPFLOW

## 1. 全局页面结构

- `/`：主页面，未登录展示简介 + 登录按钮；已登录展示：
  - 顶部：当前用户信息 + 登出。
  - 中部：发 vibe 的表单。
  - 底部：vibe 列表（全站公共）。

## 2. 典型用户路径

### 2.1 未登录用户

1. 打开 `/`。
2. 看到 hero 文案：「今天的 vibe 是什么？」＋ 登录按钮。
3. 点击「用 Google 登录」按钮。
4. 跳转到 Firebase 提供的 Google OAuth 流程。
5. 登录成功后回到 `/`，看到表单和列表。

### 2.2 已登录用户发 vibe

1. 打开 `/`（已登录自动识别）。
2. 看到一个多行文本框 + 标签选择（下拉 / 单选 chips）。
3. 输入 10～200 字的句子。
4. 选择一个标签（默认「无标签」）。
5. 点击「发射 vibe」按钮。
6. 前端调用后端 handler，写入 Firestore：
   - `userId`
   - `userName`（优先 displayName，其次 email 前缀）
   - `text`
   - `tag`
   - `createdAt`
7. 成功后清空输入框，在列表顶端看到刚刚发的内容。

### 2.3 删除自己的 vibe

1. 在公共列表中，自己发的 vibe 卡片右上角有一个「删除」icon。
2. 点击后弹出确认对话框。
3. 确认后调用后端删除 Firestore 文档。
4. 前端本地列表移除该条目。

## 3. 主要组件交互

- `AuthProvider`：包裹整个 App，监听 Firebase Auth 状态，提供 `user` context。
- `VibeForm`：
  - 依赖 `user`。
  - 调用 `createVibe`（封装 Firestore 写入）。
- `VibeList`：
  - 首次加载调用 `listVibes`（监听或一次性读取）。
  - 渲染卡片列表。
- `DeleteVibeButton`：
  - 校验当前 `user.uid === vibe.userId` 才显示。
  - 调用 `deleteVibe`。

## 4. 错误与边界情况

- 未登录用户尝试发 vibe：按钮 disabled + 提示「请先登录」。
- Firestore 写入失败：toast 提示「发送失败，请稍后重试」。
- 列表加载失败：显示「加载失败」按钮，可重试。
