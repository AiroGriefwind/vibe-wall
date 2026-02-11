# Vibe 句子收藏墙 - FILE_STRUCTURE

标准目录结构：

my-app/
├── src/
│   ├── app/                # 页面和路由（Next.js App Router）
│   │   └── page.tsx        # 首页：vibe 墙
│   ├── components/         # 可复用 UI 组件
│   │   ├── VibeForm.tsx    # 发 vibe 的表单
│   │   ├── VibeList.tsx    # vibe 列表容器
│   │   └── VibeCard.tsx    # 单条 vibe 卡片
│   ├── lib/                # 工具、数据访问
│   │   ├── firebase.ts     # Firebase 初始化
│   │   └── vibes.ts        # createVibe / listVibes / deleteVibe
│   └── styles/             # 样式文件（Tailwind 入口等）
├── public/                 # 图片、静态资源
├── .env.local              # 本地环境变量（永不提交）
├── CLAUDE.md               # AI 使用说明、约束、上下文
├── progress.txt            # 开发过程 / 对话跟踪
├── PRD.md                  # 产品需求
├── APPFLOW.md              # 用户流程和导航
├── TECHSTACK.md            # 技术栈与依赖
├── FRONTENDGUIDELINES.md   # 设计系统 / UI 规范
├── BACKENDSTRUCTURE.md     # 数据模型和「伪后端」函数说明
├── FILE_STRUCTURE.md       # 文件和目录约定（本文件）
├── IMPLEMENTATIONPLAN.md   # 实施步骤与任务列表
├── package.json            # 依赖与脚本
└── README.md               # 项目概览 + 起步指南

## AI 代码生成约定

当 AI 生成代码时，必须明确指出文件路径，并遵守上面的结构。例如：

- 「在 `src/lib/firebase.ts` 中创建或更新以下内容：」
- 「在 `src/components/VibeForm.tsx` 中放入：」
- 「修改 `src/app/page.tsx`，替换为：」

禁止：

- 在未定义的目录下随意创建文件（例如 `utils/`、`services/`）；
- 混淆 `app/` 与 `pages/` 目录。

如果需要新增目录，必须先在 `FILE_STRUCTURE.md` 中补充说明，再让 AI 使用。
