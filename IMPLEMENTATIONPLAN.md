# Vibe Wall - IMPLEMENTATION_PLAN

> 约定：
> - 使用 git worktree 把不同功能拆到不同目录并行开发。
> - 每个 worktree 对应一个 Cursor 窗口和一组专用 chat。
> - 当 AI 生成代码时，必须明确指出文件路径，并遵守 FILE_STRUCTURE.md。

## 阶段 0：初始化主仓库（main worktree）

- [ ] 新建 GitHub 仓库 `vibe-wall`.
- [ ] 在本地 clone 主仓库，例如：
  - `git clone git@github.com:me/vibe-wall.git`
- [ ] 在主目录（`vibe-wall/`）使用 `create-next-app` 创建 Next.js 14 + TypeScript 项目。
- [ ] 配置 Tailwind CSS。
- [ ] 在根目录创建：
  - `PRD.md`
  - `APPFLOW.md`
  - `TECHSTACK.md`
  - `FRONTENDGUIDELINES.md`
  - `BACKENDSTRUCTURE.md`
  - `FILE_STRUCTURE.md`
  - `IMPLEMENTATIONPLAN.md`
- [ ] 初始化 shadcn/ui（用于 Button、Badge、Dialog 等基础组件）。
- [ ] 确认 `firebase_secrets.json` 不提交到仓库（加入 .gitignore）。
- [ ] 首次提交并 push 到 GitHub：
  - `git add .`
  - `git commit -m "chore: init vibe-wall"`
  - `git push -u origin main`

> 提示：主工作树（`vibe-wall/`）只用来做基础搭建和合并，不长期写功能代码。

## 阶段 1：创建并行 worktrees

在主仓库目录 `vibe-wall/` 中执行（终端命令）：

- [ ] 创建分支并 worktree：认证系统（auth）
  - `git branch feature/auth`
  - `git worktree add ../vibe-wall-auth feature/auth`
- [ ] 创建分支并 worktree：UI 布局（ui）
  - `git branch feature/ui`
  - `git worktree add ../vibe-wall-ui feature/ui`
- [ ] 创建分支并 worktree：数据 + Firestore（data）
  - `git branch feature/data`
  - `git worktree add ../vibe-wall-data feature/data`

结果应该是三个独立目录：

- `../vibe-wall-auth`   → 负责登录 / Auth / 全局 AuthProvider
- `../vibe-wall-ui`     → 负责首页布局 + VibeForm/VibeList 的 UI
- `../vibe-wall-data`   → 负责 Firestore 数据访问、规则示例等

> 约定：任何时候，一个功能改动只在对应 worktree 里进行，完成后再合并回 `main`。

## 阶段 2：在 Cursor 中打开并行会话

- [ ] 打开 Cursor，分别以三个独立 workspace 方式打开：
  - Workspace A：目录 `vibe-wall-auth/`
  - Workspace B：目录 `vibe-wall-ui/`
  - Workspace C：目录 `vibe-wall-data/`
- [ ] 每个 workspace 建立至少一个固定对话，并在开头说明各自职责，例如：

  - **Auth 会话（A）**：
    - 只负责 Firebase 初始化、AuthProvider、登录 / 登出逻辑。
    - 只改 `src/lib/firebase.ts`、`src/components/AuthProvider.tsx`、`src/app/layout.tsx` 等与认证相关的文件。
  
  - **UI 会话（B）**：
    - 只负责页面布局和 UI 组件。
    - 只改 `src/app/page.tsx`、`src/components/VibeForm.tsx`、`src/components/VibeList.tsx`、`src/components/VibeCard.tsx` 等。
  
  - **Data 会话（C）**：
    - 只负责 Firestore 数据访问函数。
    - 只改 `src/lib/vibes.ts`、与数据相关的 hooks（如 `src/lib/hooks/useVibes.ts`）以及相关测试 / 示例。

- [ ] 在三个会话中都提示 AI：
  - 阅读项目根目录的：
    - `PRD.md`
  - `BACKENDSTRUCTURE.md`
  - `FILE_STRUCTURE.md`
  - `FRONTENDGUIDELINES.md`
  - 之后所有代码变更都必须带文件路径，例如：
    - 「在 `src/lib/firebase.ts` 中写入：…」
    - 「在 `src/components/VibeForm.tsx` 中替换为：…」

## 阶段 3：Auth worktree（feature/auth）

在 `vibe-wall-auth/` + Cursor Auth 会话中完成：

- [ ] 在 `src/lib/firebase.ts` 中初始化 Firebase app，并导出 `auth` 实例。
- [ ] 在 `src/components/AuthProvider.tsx` 中：
  - 使用 Firebase Auth 监听用户状态。
  - 通过 React Context 提供 `user`、`signInWithGoogle`、`signOut`。
- [ ] 在 `src/components/AuthStatus.tsx` 中实现登录区 UI（按钮 + 用户信息）。
- [ ] 在 `src/app/layout.tsx` 中用 `AuthProvider` 包裹应用。
- [ ] 确保所有代码路径符合 `FILE_STRUCTURE.md`。
- [ ] 本地跑一次 `npm run dev` / `pnpm dev` 验证 Auth 流程。
- [ ] 提交到 `feature/auth`：
  - `git add .`
  - `git commit -m "feat: basic Firebase auth"`

## 阶段 4：UI worktree（feature/ui）

在 `vibe-wall-ui/` + Cursor UI 会话中完成：

- [ ] 修改 `src/app/page.tsx` 实现基础布局：
  - 顶部：项目标题、副标题、登录区占位（先用简单占位容器，不接 Auth）。
  - 中部：`<VibeForm />` 组件占位。
  - 底部：`<VibeList />`（先用 mock 数据）。
- [ ] 创建 UI 组件：
  - `src/components/VibeForm.tsx`：表单 UI，暂时只做 `console.log`。
  - `src/components/VibeList.tsx`：接收 `vibes` 数组，循环渲染 `VibeCard`。
  - `src/components/VibeCard.tsx`：单条 vibe 卡片样式。
- [ ] 按 `FRONTEND_GUIDELINES.md` 调整配色、间距、字体。
- [ ] 用前端 mock 数据填充列表，确保 UI 视觉没问题。
- [ ] 提交到 `feature/ui`：
  - `git add .`
  - `git commit -m "feat: base layout and mock UI"`

## 阶段 5：Data worktree（feature/data）

在 `vibe-wall-data/` + Cursor Data 会话中完成：

- [ ] 在 `src/lib/vibes.ts` 中实现：
  - `createVibe(user, text, tag)`
  - `listVibes(limit = 50)`
  - `deleteVibe(user, id)`
- [ ] 增加 Firestore 安全规则说明（仅登录可写；仅作者可删；公开可读）。
- [ ] 如需 hook，可在 `src/lib/hooks/useVibes.ts` 中实现 `useVibes`（并在 `FILE_STRUCTURE.md` 中登记）。
- [ ] 确保数据模型与 `BACKEND_STRUCTURE.md` 一致。
- [ ] 可以写一个简单的「测试页」或临时 script 验证 Firestore 读写。
- [ ] 提交到 `feature/data`：
  - `git add .`
  - `git commit -m "feat: firestore data layer"`

## 阶段 6：合并并行成果回 main

在主仓库 `vibe-wall/` 中进行（非 worktree 目录）：

- [ ] 依次合并分支：
  - `git checkout main`
  - `git merge feature/auth`
  - `git merge feature/ui`
  - `git merge feature/data`
- [ ] 解决可能的冲突，优先遵守：
  - `FILE_STRUCTURE.md` 中的路径约定。
  - `BACKEND_STRUCTURE.md` 中的数据模型。
- [ ] 在最终合并后的 main 里补齐首页登录区：把占位替换为 `AuthStatus`。
- [ ] 在最终合并后的 main 里，用 Cursor 开一个新的「总控」会话，检查：
  - 登录、发 vibe、列表展示、删除功能是否连通。
- [ ] 提交并 push：
  - `git add .`
  - `git commit -m "chore: merge parallel worktrees into main"`
  - `git push`

## 阶段 7：部署到 Vercel（基于 main）

- [ ] 使用 `main` 分支连接 Vercel。
- [ ] 在 Vercel 设置环境变量（与 `.env.local` 一致）。
- [ ] 部署后用浏览器验证基本流程：

  - 未登录访问。
  - 登录。
  - 发 vibe。
  - 看到列表。
  - 删除自己的 vibe。

## 阶段 8：复盘与优化

- [ ] 在 `progress.txt` 中记录：
  - 三个 worktree 并行开发的体验。
  - Cursor 在不同 worktree 会话中的表现。
  - 哪些任务适合并行，哪些还是顺序更稳。
- [ ] 如需继续扩展（统计页、多用户视图等），重复：
  - 新建分支 + 新 worktree。
  - 新 Cursor 会话。
  - 在 `FILE_STRUCTURE.md` 和 `IMPLEMENTATION_PLAN.md` 中登记。