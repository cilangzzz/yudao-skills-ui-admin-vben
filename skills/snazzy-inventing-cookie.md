# 目录结构解析报告

## 项目概述

| 属性 | 值 |
|------|-----|
| 项目名称 | yudao-ui-admin-vben |
| 版本 | 5.7.0 |
| 架构类型 | Monorepo (pnpm workspace + Turborepo) |
| 前端框架 | Vue 3.5.30 |
| 构建工具 | Vite 8.0.1 |
| 包管理器 | pnpm 10.32.1 |

---

## 1. 目录结构总览

```
yudao-ui-admin-vben/
├── apps/                    # 应用层 - 多UI框架实现
│   ├── web-antd/           # Ant Design Vue 版本
│   ├── web-antdv-next/     # Antdv Next 版本
│   ├── web-ele/            # Element Plus 版本
│   ├── web-naive/          # Naive UI 版本
│   └── web-tdesign/        # TDesign 版本
│
├── packages/               # 包层 - 共享模块
│   ├── @core/             # 核心基础包
│   │   ├── base/          # 基础设计、图标、类型
│   │   ├── composables/   # 组合式函数
│   │   ├── preferences/    # 偏好设置
│   │   └── ui-kit/        # UI组件套件
│   ├── effects/           # 效应层
│   │   ├── access/        # 权限控制
│   │   ├── common-ui/     # 通用UI组件
│   │   ├── hooks/         # 自定义Hooks
│   │   ├── layouts/       # 布局组件
│   │   ├── plugins/       # 插件系统
│   │   └── request/       # 请求封装
│   ├── constants/         # 常量定义
│   ├── icons/             # 图标资源
│   ├── locales/           # 国际化
│   ├── preferences/       # 用户偏好
│   ├── stores/            # Pinia状态管理
│   ├── styles/            # 全局样式
│   ├── types/             # TypeScript类型
│   └── utils/             # 工具函数
│
├── internal/              # 内部工具
│   ├── lint-configs/      # Lint配置集合
│   │   ├── commitlint-config/
│   │   ├── eslint-config/
│   │   ├── oxfmt-config/
│   │   ├── oxlint-config/
│   │   └── stylelint-config/
│   ├── node-utils/        # Node工具
│   ├── tailwind-config/   # Tailwind配置
│   ├── tsconfig/          # TypeScript配置
│   └── vite-config/       # Vite配置
│
├── scripts/               # 脚本工具
│   ├── deploy/            # 部署脚本
│   ├── turbo-run/         # Turbo运行器
│   └── vsh/               # 内部CLI工具
│
├── docs/                  # VitePress文档站点
│   └── .vitepress/
│
└── 配置文件
    ├── package.json       # 根项目配置
    ├── pnpm-workspace.yaml # Workspace配置
    ├── turbo.json         # Turborepo配置
    └── vitest.config.ts   # 测试配置
```

---

## 2. 架构分层设计

### 2.1 应用层 (apps/)
多UI框架并行实现，共享核心逻辑：

| 应用 | UI框架 | 说明 |
|------|--------|------|
| web-antd | Ant Design Vue 4.2.6 | 主要推荐版本 |
| web-antdv-next | antdv-next 1.1.5 | 新版预览 |
| web-ele | Element Plus 2.13.5 | Element生态 |
| web-naive | Naive UI 2.44.1 | 轻量级UI |
| web-tdesign | TDesign 1.18.5 | 腾讯设计体系 |

### 2.2 核心层 (packages/@core/)
```
packages/@core/
├── base/
│   ├── design/       # 设计系统基础
│   ├── icons/        # 基础图标
│   ├── shared/       # 共享工具
│   └── typings/      # 类型定义
├── composables/      # 组合式API
├── preferences/      # 偏好设置系统
└── ui-kit/          # 无UI框架依赖的组件
    ├── form-ui/     # 表单UI
    ├── layout-ui/   # 布局UI
    ├── menu-ui/     # 菜单UI
    ├── popup-ui/    # 弹窗UI
    ├── shadcn-ui/   # Shadcn风格组件
    └── tabs-ui/     # 标签页UI
```

### 2.3 效应层 (packages/effects/)
跨应用共享的业务效应：

| 模块 | 职责 |
|------|------|
| access | 权限控制、路由守卫 |
| common-ui | 通用业务组件 |
| hooks | 自定义组合式函数 |
| layouts | 页面布局模板 |
| plugins | 插件系统架构 |
| request | Axios封装、请求拦截 |

---

## 3. 技术栈分析

### 3.1 核心技术

| 层级 | 技术 | 版本 | 用途 |
|------|------|------|------|
| 框架 | Vue | 3.5.30 | 前端框架 |
| 构建 | Vite | 8.0.1 | 开发与构建 |
| 类型 | TypeScript | 5.9.3 | 类型安全 |
| 状态 | Pinia | 3.0.4 | 状态管理 |
| 路由 | Vue Router | 5.0.3 | 路由管理 |
| 样式 | Tailwind CSS | 4.2.2 | 原子化CSS |

### 3.2 Monorepo工具链

| 工具 | 用途 |
|------|------|
| pnpm workspace | 包管理与依赖 |
| Turborepo | 构建缓存与编排 |
| Changesets | 版本管理与发布 |

### 3.3 代码质量

| 工具 | 用途 |
|------|------|
| ESLint | 代码检查 |
| Oxlint | 高性能Lint |
| Stylelint | 样式检查 |
| Commitlint | 提交规范 |
| Lefthook | Git Hooks |
| Vitest | 单元测试 |
| Playwright | E2E测试 |

---

## 4. 应用目录结构 (以web-antd为例)

```
apps/web-antd/
├── public/              # 静态资源
│   ├── static/
│   └── tinymce/        # 富文本编辑器
│
├── src/
│   ├── adapter/        # 适配器层
│   ├── api/            # API接口定义
│   ├── assets/         # 资源文件
│   ├── components/     # 业务组件
│   ├── layouts/        # 布局组件
│   ├── locales/        # 国际化资源
│   ├── plugins/        # 插件配置
│   ├── router/         # 路由配置
│   ├── store/          # 状态存储
│   ├── utils/          # 工具函数
│   └── views/          # 页面视图
│
└── package.json
```

---

## 5. 架构设计特点

### 5.1 优点

1. **UI框架解耦**: 通过ui-kit层抽象，实现多UI框架支持
2. **共享逻辑复用**: packages层提供跨应用的业务逻辑复用
3. **构建优化**: Turborepo实现增量构建和缓存
4. **类型安全**: 全TypeScript覆盖，完整类型定义
5. **代码规范**: 完善的Lint体系和Git Hooks

### 5.2 架构模式

```
┌─────────────────────────────────────────────────────┐
│                    apps/ (应用层)                    │
│  web-antd  web-ele  web-naive  web-tdesign         │
├─────────────────────────────────────────────────────┤
│                packages/effects/ (效应层)           │
│   access  common-ui  hooks  layouts  plugins       │
├─────────────────────────────────────────────────────┤
│                packages/@core/ (核心层)             │
│   base  composables  preferences  ui-kit           │
├─────────────────────────────────────────────────────┤
│                packages/ (基础设施层)               │
│   constants  icons  locales  stores  styles  utils │
├─────────────────────────────────────────────────────┤
│                internal/ (工具配置层)               │
│   lint-configs  vite-config  tsconfig              │
└─────────────────────────────────────────────────────┘
```

---

## 6. 研发建议

### 6.1 开发命令

```bash
# 安装依赖
pnpm install

# 启动开发 (Ant Design版本)
pnpm dev:antd

# 启动开发 (Element Plus版本)
pnpm dev:ele

# 构建生产版本
pnpm build:antd

# 代码检查
pnpm lint

# 类型检查
pnpm check:type
```

### 6.2 新功能开发流程

1. **共享逻辑** → 添加到 `packages/effects/` 或 `packages/`
2. **UI无关组件** → 添加到 `packages/@core/ui-kit/`
3. **业务页面** → 添加到 `apps/web-antd/src/views/`
4. **API接口** → 添加到对应应用的 `src/api/`

---

## 7. 相关文档路径

| 文档类型 | 路径 |
|----------|------|
| 项目说明 | README.md |
| 构建配置 | turbo.json |
| 包管理 | pnpm-workspace.yaml |
| 开发文档 | docs/src/guide/ |

---

*报告生成时间: 2026-03-25*
*使用研发部 architect skill 分析*

---

## 附录：前端仓库解析提示词（多Agent Team 格式）

### 使用方式

将以下提示词复制到 Claude Code 中启动多Agent团队解析：

```markdown
# 前端仓库解析任务 - 多Agent Team 协作

## 任务目标
为 yudao-ui-admin-vben 前端仓库创建完整的 Skill 文档，形成可复用的知识资产。

## 项目信息
- 项目路径: h:\Documents\yudao-ui-admin-vben-master\yudao-ui-admin-vben-master
- 架构类型: Monorepo (pnpm workspace + Turborepo)
- 前端框架: Vue 3.5.30 + Vite 8.0.1
- UI框架: Ant Design Vue / Element Plus / Naive UI / TDesign

## 团队配置

请使用 TeamCreate 创建团队，然后并行启动以下 5 个 Agent 进行解析：

### Agent 1: 架构分析师 (architect-analyst)
**Subagent Type**: Explore
**任务**: 分析项目整体架构设计
- 解析 Monorepo 架构设计（pnpm workspace + Turborepo）
- 分析 apps/ 和 packages/ 的分层设计
- 识别架构设计模式和设计决策
- 输出: 架构设计文档

**关键文件**:
- package.json
- pnpm-workspace.yaml
- turbo.json
- apps/*/
- packages/*/

### Agent 2: 核心包分析师 (core-analyst)
**Subagent Type**: Explore
**任务**: 分析核心包 (@core) 的设计
- 分析 packages/@core/base/ 基础设计
- 分析 packages/@core/composables/ 组合式函数
- 分析 packages/@core/ui-kit/ UI组件抽象
- 分析 packages/effects/ 效应层设计
- 输出: 核心包设计文档

**关键文件**:
- packages/@core/base/
- packages/@core/composables/
- packages/@core/ui-kit/
- packages/effects/

### Agent 3: 应用层分析师 (app-analyst)
**Subagent Type**: Explore
**任务**: 分析应用层实现
- 分析 apps/web-antd/ 目录结构
- 分析路由、状态管理、API层设计
- 分析页面组件和布局设计
- 输出: 应用层设计文档

**关键文件**:
- apps/web-antd/src/
- apps/web-antd/src/router/
- apps/web-antd/src/store/
- apps/web-antd/src/api/

### Agent 4: 组件规范分析师 (component-analyst)
**Subagent Type**: Explore
**任务**: 分析组件和代码规范
- 分析业务组件设计模式
- 分析表单、表格、弹窗等通用组件
- 分析代码风格和命名规范
- 输出: 组件规范文档

**关键文件**:
- apps/web-antd/src/components/
- packages/effects/common-ui/

### Agent 5: 技术栈分析师 (tech-analyst)
**Subagent Type**: Explore
**任务**: 分析技术栈和工具链
- 分析 Vue 3 + TypeScript + Vite 配置
- 分析 Tailwind CSS 样式方案
- 分析 Lint 工具链配置
- 分析国际化、权限、请求封装
- 输出: 技术栈文档

**关键文件**:
- internal/lint-configs/
- packages/locales/
- packages/effects/request/
- packages/effects/access/

## 输出格式

最终输出目录结构:
```
skills/
├── index.yaml                    # Skill 索引文件
├── templates/
│   └── extraction-prompt.md      # 提取提示词模板
├── modules/
│   ├── core/
│   │   └── skill-core.yaml       # 核心包 Skill
│   ├── effects/
│   │   └── skill-effects.yaml    # 效应层 Skill
│   ├── app-antd/
│   │   └── skill-app-antd.yaml   # Ant Design 应用 Skill
│   ├── app-ele/
│   │   └── skill-app-ele.yaml    # Element Plus 应用 Skill
│   └── components/
│       └── skill-components.yaml # 组件 Skill
└── usage/
    ├── quick-start.md            # 快速上手指南
    ├── new-feature.md            # 新增功能指南
    └── pattern-usage.md          # 模式使用指南
```

## 分析框架（五阶段）

每个模块的分析应遵循以下框架：

### 第一阶段：设计理念提取
1. 业务定位：模块解决什么问题？
2. 设计原则：遵循了哪些设计原则？
3. 技术选型：为什么选择这些技术？

### 第二阶段：架构设计提取
1. 目录结构分析
2. 分层架构设计
3. 模块间通信方式

### 第三阶段：核心实现提取
1. 关键类型定义
2. 核心函数和组件
3. 配置和约定

### 第四阶段：代码使用设计
1. 组件使用规范
2. API调用规范
3. 样式编写规范

### 第五阶段：扩展指南
1. 新增功能步骤
2. 新增组件示例
3. 最佳实践总结

## 执行指令

请先创建团队，然后并行启动 5 个 Agent，最后汇总输出完整的 Skill 文档。
```

---

### Skill 文档模板 (skill-template.yaml)

```yaml
# Skill 模板文件 - 前端版本
# 用于提取模块知识的标准格式

skill:
  id: "skill-{module}"
  name: "{模块名称} Skill"
  version: "1.0.0"
  module_path: "{模块路径}"
  created_at: ""
  updated_at: ""

# ============================================
# 第一阶段：设计理念
# ============================================
philosophy:
  # 业务定位
  business_position: ""

  # 设计原则
  design_principles: []

  # 技术选型理由
  tech_choices: []

# ============================================
# 第二阶段：架构设计
# ============================================
architecture:
  # 目录结构
  directory_structure:
    layers: []

  # 模块依赖
  dependencies:
    internal: []
    external: []

  # 设计模式
  design_patterns: []

# ============================================
# 第三阶段：核心实现
# ============================================
implementation:
  # 类型定义
  types: []

  # 核心组件
  components: []

  # 核心函数
  functions: []

  # 配置项
  configurations: []

# ============================================
# 第四阶段：代码使用设计
# ============================================
code_patterns:
  # 组件使用规范
  component_usage:
    example: ""

  # API调用规范
  api_usage:
    example: ""

  # 样式规范
  style_usage:
    example: ""

  # 命名规范
  naming_conventions: []

# ============================================
# 第五阶段：扩展指南
# ============================================
extension_guide:
  # 新增功能步骤
  new_feature:
    steps: []

  # 新增组件示例
  new_component:
    steps: []

  # 最佳实践
  best_practices: []

# ============================================
# 关键文件清单
# ============================================
key_files:
  - path: ""
    purpose: ""
```

---

*提示词生成时间: 2026-03-25*