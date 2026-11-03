# 应用大纲（模仿 Microsoft Planner）

> 本文档是「Planner React 克隆版」的总体大纲，定义应用的目标、功能模块、页面结构、数据模型与组件划分。它是 README、AGENTS 与开发计划的事实来源（Source of Truth）。后续功能需求如有变化，应优先更新本大纲。

## 1. 目标与定位

用 React 开发一套与 Microsoft Planner 功能对齐的轻量团队任务管理 Web 应用，支持：

- 以「计划（Plan）」为单位组织工作；
- 在看板（Board）中按「存储桶（Bucket）」分列管理任务（Task）；
- 用图表（Charts）和日程（Schedule）多视角查看进度与工作量；
- 通过「我的任务（My Tasks）」聚合个人待办；
- 支持任务详情、清单、标签、优先级、截止日期、附件、评论与成员分配等核心能力。

原则：**先做可用的本地单机版（数据持久化在浏览器），预留服务端数据接口，后续可平滑接入后端。**

## 2. 核心概念与数据模型

### 2.1 实体

| 实体 | 说明 | 关键字段 |
| --- | --- | --- |
| Plan（计划） | 工作集合，包含若干存储桶与任务 | id、title、owner、members、createdAt、updatedAt |
| Bucket（存储桶） | 看板中的一列 | id、planId、name、order |
| Task（任务） | 卡片，最小的可执行工作单元 | id、planId、bucketId、title、notes、progress、priority、startDate、dueDate、labels、order、assigneeIds、checklist、attachments、comments、createdAt、updatedAt |
| Member（成员） | 计划中的参与者 | id、planId、name、email、avatar、color |
| Label（标签） | 彩色标记，可多选 | id、planId、name、color |
| ChecklistItem（清单项） | 任务内的勾选项 | id、taskId、title、checked、order |
| Comment（评论） | 任务活动流中的留言 | id、taskId、authorId、body、createdAt |
| Attachment（附件） | 任务附件引用 | id、taskId、name、url、type、size |

### 2.2 关键枚举

- **progress（进度）**：`notStarted`（未开始）→ `inProgress`（进行中）→ `completed`（已完成）
- **priority（优先级）**：`urgent`（紧急）、`important`（重要）、`medium`（中等）、`low`（低）

## 3. 页面结构

| 路由 | 页面 | 说明 |
| --- | --- | --- |
| `/` | 计划中心（Plan Hub） | 展示全部计划、最近访问，支持新建/删除/重命名计划 |
| `/plan/:planId/board` | 看板视图（Board） | Kanban 看板：按存储桶分列展示任务卡片，支持拖拽 |
| `/plan/:planId/charts` | 图表视图（Charts） | 按状态/存储桶/优先级/成员统计任务的柱状图与饼图 |
| `/plan/:planId/schedule` | 日程视图（Schedule） | 以日历/周视图展示有截止日期的任务 |
| `/my-tasks` | 我的任务（My Tasks） | 聚合当前用户在所有计划中被分配的任务 |

## 4. 功能模块

### 4.1 计划管理
- 新建、删除、重命名计划；设置计划标题。
- 成员管理：添加/移除成员，成员用于任务分配与统计。

### 4.2 看板（Board）
- 新建/重命名/删除存储桶（Bucket）。
- 在存储桶中新建任务卡片。
- 任务卡片展示：标题、进度、优先级、截止日期、标签、成员头像、清单完成度。
- 拖拽任务卡片到不同存储桶，或在同一存储桶内排序。
- 按关键字过滤、按字段分组。

### 4.3 任务详情
点击卡片打开详情面板（或弹窗），支持编辑：
- 标题与备注（notes）；
- 进度、优先级、开始/截止日期；
- 标签、成员分配；
- 清单（Checklist）：添加/勾选/删除；
- 附件：上传/引用文件；
- 评论与活动流。

### 4.4 图表视图（Charts）
- 状态分布（未开始/进行中/已完成/逾期）。
- 存储桶分布、优先级分布、成员工作量分布。

### 4.5 日程视图（Schedule）
- 月/周切换；任务按开始-截止日期渲染为日历条目。
- 点击条目跳转任务详情。

### 4.6 我的任务（My Tasks）
- 跨计划聚合分配给当前用户的任务。
- 支持按计划、状态、日期过滤与快速完成。

### 4.7 通用能力
- 搜索：按任务标题/成员/标签搜索。
- 通知（本地模拟）：任务分配、逾期提醒的站内提示。
- 深色/浅色主题、响应式布局、键盘可达性。

## 5. 组件划分（高层）

| 组件 | 职责 |
| --- | --- |
| `AppShell` / `Layout` | 顶栏、左侧导航、路由出口 |
| `PlanHub` | 计划中心页 |
| `BoardView` | 看板容器，管理存储桶与拖拽 |
| `BucketColumn` | 单个存储桶列，含任务卡片列表 |
| `TaskCard` | 任务卡片 |
| `TaskDetailPanel` | 任务详情编辑面板 |
| `ChecklistEditor` | 清单编辑器 |
| `LabelPicker` / `DatePicker` / `PriorityPicker` / `AssigneePicker` | 各字段选择器 |
| `ChartsView` / `ScheduleView` / `MyTasksView` | 各视图页 |
| `DnDContext` | 拖拽上下文与排序逻辑 |

## 6. 技术选型（初步）

| 领域 | 选择 | 理由 |
| --- | --- | --- |
| 框架 | React 18 + TypeScript | 用户指定 React；TS 保证类型安全 |
| 构建 | Vite | 开发体验好、启动快 |
| 路由 | React Router | 标准 SPA 路由 |
| 状态管理 | Zustand（配合 Immer） | 轻量、易组合、适合本地数据 |
| 拖拽 | @dnd-kit | 现代、可访问、维护活跃 |
| 样式 | Tailwind CSS | 原子化、易主题化 |
| 日期 | date-fns | 轻量、可树摇 |
| 持久化 | localStorage / IndexedDB（先），预留 repository 接口 | MVP 无需后端 |
| 测试 | Vitest + React Testing Library + Playwright | 单元/组件/E2E 全覆盖 |
| 校验 | Zod | schema 校验数据模型 |

## 7. 边界与非目标（当前阶段）

- 不实现真实的多用户实时协作与权限（MVP 单机模拟）；
- 不实现 Office 365 Group / Microsoft 账号集成；
- 不实现邮件/推送通知服务端（仅本地模拟）；
- 附件仅做引用与本地存储，不做云端存储。

> 以上边界在接入后端后可扩展，具体以 `docs/development-plan.md` 的分阶段计划为准。
