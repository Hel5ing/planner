# AGENTS.md

本文件为在此仓库中工作的 AI 代理与人类贡献者提供开发规范。任何自动化代理（包括 GitHub Copilot、代码评审机器人、以及其他 AI）在修改本仓库前都应遵循以下约定。

## 项目概况

本项目用 React 复刻 Microsoft Planner。权威的领域模型、页面与功能定义见 [`docs/outline.md`](docs/outline.md)；阶段化开发顺序与验收标准见 [`docs/development-plan.md`](docs/development-plan.md)。这两个文档是需求的事实来源，代码实现必须与其保持一致。

## 技术栈约定

- 语言：TypeScript（严格模式），禁止使用 `any`（如确有必要需注明原因）。
- 框架：React 18，函数组件 + Hooks，不使用类组件。
- 状态：全局状态使用 Zustand（配合 Immer 做不可变更新）；局部 UI 状态用 `useState`/`useReducer`。
- 样式：Tailwind CSS 原子类，避免手写散落 CSS；主题色集中管理。
- 数据校验：数据模型用 Zod 定义 schema，校验入口数据。
- 拖拽：统一使用 @dnd-kit，禁止引入其他拖拽库。

## 目录与命名规范

- 业务代码按功能域（feature）组织于 `src/features/`，跨域复用的 UI 放 `src/components/`。
- 组件文件命名 `PascalCase.tsx`；hooks 命名 `useXxx.ts`；工具函数 `camelCase.ts`。
- 每个 feature 目录内允许 `components/`、`hooks/`、`store.ts`、`types.ts` 子结构，但不可跨 feature 直接 import 内部文件。
- 类型定义集中在 `src/models/`，供全项目复用。

## 开发流程

1. 先阅读 `docs/outline.md` 与 `docs/development-plan.md`，确认改动属于当前阶段的验收范围。
2. 数据模型或页面结构发生变化时，**必须同步更新** `docs/outline.md`。
3. 每个改动都应附带相应测试：
   - 纯逻辑与 store：Vitest 单元测试；
   - 组件：React Testing Library 组件测试；
   - 跨页面关键流程：Playwright E2E 测试。
4. 提交信息使用 Conventional Commits 格式（`feat:`、`fix:`、`docs:`、`test:`、`refactor:` 等）。
5. 通过 `pnpm lint`、`pnpm test`、`pnpm build` 后再推送。

## 代码质量

- 保持单一职责：组件只做渲染与组合，业务逻辑放入 hooks 或 store。
- 列表渲染必须使用稳定的 `key`；避免在渲染期创建新对象/函数引起无谓重渲染（必要时用 `useMemo`/`useCallback`）。
- 可访问性：交互元素可用键盘操作，焦点管理正确，颜色对比度达标。
- 日期与时间使用 `date-fns`，不要自行拼接时区逻辑。

## 数据与持久化边界

- 所有数据读写统一走 `src/repository/` 的接口，业务代码不得直接访问 `localStorage`/`IndexedDB`。
- MVP 阶段数据本地持久化；接口设计需为后续接入后端预留（异步、可替换实现）。

## 禁止事项

- 不引入与既有技术栈重复的依赖（如额外引入 Redux、MobX、react-beautiful-dnd）。
- 不在未更新大纲的情况下新增影响领域模型或页面结构的“隐性”需求。
- 不把密钥、真实用户数据或内部 Feishu 内容提交进仓库。
