# Vibe Wall - TECHSTACK

## 1. 前端

- 框架：Next.js 14（App Router，TypeScript）。
- 语言：TypeScript 5.x。
- UI 样式：Tailwind CSS 3.x。
- UI 组件库：shadcn/ui（Button、Input、Textarea、Dialog、Badge）。

## 2. 后端（BaaS）

- Firebase Auth
  - 登录方式：Google OAuth。
- Firebase Firestore
  - 用作主数据库，存储 vibe 记录。
- （可选）Firebase Hosting 不使用，由 Vercel 负责前端托管。

## 3. 部署与运维

- 部署平台：Vercel。
- CI：连接 GitHub 仓库，push 到 main 触发自动部署。
- 环境变量：
  - `NEXT_PUBLIC_FIREBASE_API_KEY`
  - `NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN`
  - `NEXT_PUBLIC_FIREBASE_PROJECT_ID`
  - `NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET`（可选）
  - `NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID`（可选）
  - `NEXT_PUBLIC_FIREBASE_APP_ID`

## 4. 开发工具

- 包管理：pnpm / npm（二选一，保持一致）。
- 代码编辑器：Cursor / VS Code。
- Lint & Format：ESLint + Prettier（可用 Next 默认配置）。
- Git 托管：GitHub。

## 5. 依赖清单（核心）

- `next`
- `react`
- `react-dom`
- `typescript`
- `tailwindcss`
- `firebase`
- `@radix-ui/react-*`（shadcn/ui 依赖）
- `clsx` + `tailwind-merge`（类名合并）
