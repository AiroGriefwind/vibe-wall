# Vibe Wall - FRONTENDGUIDELINES

## 1. 设计风格

- 基调：轻量、安静、带一点 cyber 感的「深色卡片」。
- 背景：深灰（如 `#020617` / `bg-slate-950`）。
- 主要前景：卡片使用 `bg-slate-900` + `border-slate-800`。
- 主题色：蓝色（如 `#3B82F6`，Tailwind `blue-500`），用于按钮、强调文字。
- 字体：系统字体即可 + 稍微加大行距。

## 2. 组件布局

- 容器宽度：最大宽度 `max-w-2xl`，居中。
- 间距系统：使用 8px 系列（Tailwind 的 `2, 4, 6, 8` 等）。
- 表单区：
  - 文本框：`<Textarea>`，高度适中（3–5 行）。
  - 标签选择：简单的 `select` 或一排 `Badge`+`Button`。
- 列表区：
  - 每条 vibe 是一个卡片：
    - 上：文本。
    - 下：标签 + 用户名 + 时间。

## 3. 响应式规则

- 手机为优先：
  - 默认布局为单列。
  - 字号略大一些，例如正文 `text-sm` / `text-base`。
- 桌面：
  - 容器加 `max-w-2xl`，左右有足够留白。
  - 可以轻微增加行宽。

## 4. 状态与反馈

- 按钮：
  - Loading 状态显示 Spinner / 「发送中…」。
  - Disabled 状态颜色变淡（`opacity-60`）。
- 错误信息：
  - 用 toast 或简单的红色文本在表单下方显示。
- 空状态：
  - 当没有任何 vibe 时显示一条文案：「还没有人发 vibe，开个头？」。

## 5. 代码风格

- 尽量把 UI 组件拆小：
  - `VibeForm.tsx`
  - `VibeList.tsx`
  - `VibeCard.tsx`
- 每个组件保持「一件事」：
  - 表单只管 UI + 调用提交函数。
  - 数据逻辑用 hooks 封装（比如 `useVibes`）。
