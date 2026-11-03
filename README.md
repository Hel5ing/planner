# Planner（React 克隆版）

用 React 开发的一套模仿 [Microsoft Planner](https://www.microsoft.com/microsoft-365/business/task-management-software) 的轻量团队任务管理 Web 应用：以「计划（Plan）→ 存储桶（Bucket）→ 任务（Task）」三级结构组织工作，提供看板、图表、日程、我的任务四种视图，以及任务详情、清单、标签、优先级、截止日期、成员分配、附件与评论等核心能力。

> 当前为**规划与文档阶段**，应用代码尚未实现。开发路线见 [`docs/development-plan.md`](docs/development-plan.md)。

## 文档索引

| 文档 | 说明 |
| --- | --- |
| [`docs/outline.md`](docs/outline.md) | 应用大纲：目标、数据模型、页面结构、功能模块、组件划分与技术选型 |
| [`docs/development-plan.md`](docs/development-plan.md) | 开发计划：分阶段里程碑、任务清单与验收标准 |
| [`AGENTS.md`](AGENTS.md) | 面向 AI 协作代理与贡献者的开发规范 |

## 核心功能

- **看板视图**：按存储桶分列的 Kanban 看板，支持拖拽任务卡片排序与跨列移动。
- **任务详情**：标题、备注、进度、优先级、起止日期、彩色标签、成员分配、清单、附件与评论。
- **图表视图**：按状态、存储桶、优先级、成员统计任务分布。
- **日程视图**：月/周日历展示有截止日期的任务。
- **我的任务**：跨计划聚合分配给当前用户的任务。
- **计划中心**：新建、删除、重命名计划并管理成员。

## 技术栈

- **框架**：React 18 + TypeScript
- **构建**：Vite
- **路由**：React Router
- **状态管理**：Zustand + Immer
- **拖拽**：@dnd-kit
- **样式**：Tailwind CSS
- **日期**：date-fns
- **测试**：Vitest + React Testing Library + Playwright
- **数据校验**：Zod

## 快速开始

```bash
# 安装依赖
pnpm install

# 启动开发服务器
pnpm dev

# 构建生产版本
pnpm build

# 运行测试
pnpm test
```

> 脚手架尚未创建，以上命令为开发计划中的约定命令，将在 Phase 0 落地。

## 项目结构（规划）

```
.
├── docs/
│   ├── outline.md               # 应用大纲
│   └── development-plan.md      # 开发计划
├── src/
│   ├── components/              # 通用 UI 组件
│   ├── features/                # 按功能域组织的页面与逻辑
│   │   ├── plan/                # 计划中心
│   │   ├── board/               # 看板
│   │   ├── task/                # 任务详情
│   │   ├── charts/              # 图表
│   │   ├── schedule/            # 日程
│   │   └── my-tasks/            # 我的任务
│   ├── models/                  # 类型定义与数据校验
│   ├── stores/                  # Zustand 状态
│   ├── repository/              # 数据持久化接口
│   ├── routes/                  # 路由配置
│   └── styles/                  # 全局样式
├── tests/                       # 测试
├── AGENTS.md
└── README.md
```

## 许可

[MIT](LICENSE)
