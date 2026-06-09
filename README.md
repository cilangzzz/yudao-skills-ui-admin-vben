# yudao-skills-ui-admin-vben

> 基于 yudao-ui-admin-vben 的 AI 辅助开发知识库，通过 Skill 文档体系实现前端标准化代码生成。

## 🎯 项目特色

本仓库在芋道 Vben5 前端源码基础上，构建了一套完整的 **AI Skill 前端开发规范体系**，让 AI 能够：

- 理解 Monorepo 架构和分层设计
- 自动生成符合项目标准的前端代码
- 遵循 BEM 命名、适配器模式、无头组件等最佳实践
- 支持多 UI 框架（Ant Design Vue / Element Plus / Naive UI / TDesign）

## 🆚 与上游仓库的区别

本仓库基于 [yudaocode/yudao-ui-admin-vben](https://gitee.com/yudaocode/yudao-ui-admin-vben) 同步代码，在其基础上新增了 AI Skill 知识库体系。上游代码保持原样，以下为本仓库独有的内容：

| 新增内容 | 路径 | 说明 |
|---------|------|------|
| **模块 Skill** | `skills/modules/` | 核心包、效应层、Ant Design 应用 3 个模块的结构化 Skill YAML |
| **Skill 模板** | `skills/templates/` | 新模块 Skill 编写模板 |
| **UI 开发指南** | `skills/ui/` | TDesign 等 UI 框架的开发规范 |
| **使用指南** | `skills/usage/` | 快速上手文档 |
| **Skill 索引** | `skills/index.yaml` | 所有模块 Skill 的注册与状态追踪 |

> 同步方式：定期从 upstream/master 拉取最新代码合并，自定义文件始终保留。

## 📁 Skill 体系结构

```
skills/
├── index.yaml                 # Skill 索引（模块注册、状态追踪）
├── modules/                   # 模块 Skill
│   ├── core/                 # 核心包 (@core)
│   │   └── skill-core.yaml   #   base / composables / preferences / ui-kit
│   ├── effects/              # 效应层
│   │   └── skill-effects.yaml#   access / request / hooks / layouts / plugins
│   └── app-antd/             # Ant Design Vue 应用
│       └── skill-app-antd.yaml#  路由 / 状态管理 / API层 / 页面结构
├── templates/                 # Skill 编写模板
│   └── skill-template.yaml
├── ui/                        # UI 框架开发指南
│   └── frame/tdesgin/
└── usage/                     # 使用指南
    └── quick-start.md
```

## 🚀 核心能力

### 1. 分层架构 Skill

| 层级 | 模块路径 | 说明 |
|-----|---------|------|
| 核心层 | `packages/@core` | UI 框架无关的基础层：设计系统、组合式函数、偏好设置、无头组件 |
| 效应层 | `packages/effects` | 框架效应层：权限控制、请求封装、布局系统、插件机制 |
| 应用层 | `apps/web-antd` | Ant Design Vue 主应用：路由、状态管理、API 模块、业务页面 |

### 2. 设计原则

| 原则 | 说明 |
|-----|------|
| UI 框架无关 | 核心组件不依赖具体 UI 库，通过抽象层支持多框架 |
| 无头组件模式 | 使用 reka-ui 提供可访问性和交互逻辑，样式由具体 UI 层提供 |
| BEM 命名规范 | CSS 类名采用 `Block__Element--Modifier` 命名 |
| 适配器模式 | 通过适配器层统一不同 UI 框架的组件接口 |
| 状态分层管理 | 框架层 Store 和应用层 Store 分离 |

### 3. Monorepo 架构

```
├── apps/                    # 应用层
│   ├── web-antd/           # Ant Design Vue 版本
│   ├── web-ele/            # Element Plus 版本
│   ├── web-naive/          # Naive UI 版本
│   └── web-tdesign/        # TDesign 版本
├── packages/                # 包层
│   ├── @core/              # 核心包（UI 无关）
│   ├── effects/            # 效应层
│   └── constants/          # 常量包
├── scripts/                 # 工具脚本
└── internal/                # 内部构建工具
```

## 🛠️ 技术栈

| 框架 | 说明 | 版本 |
| --- | --- | --- |
| [Vue](https://staging-cn.vuejs.org/) | vue框架 | 3.5.35 |
| [Vite](https://cn.vitejs.dev//) | 开发与构建工具 | 8.0.10 |
| [Ant Design Vue](https://www.antdv.com/) | Ant Design Vue | 4.2.6 |
| [Element Plus](https://element-plus.org/zh-CN/) | Element Plus | 2.14.1 |
| [Naive UI](https://www.naiveui.com/) | Naive UI | 2.44.1 |
| [TDesign](https://tdesign.tencent.com/) | TDesign | 1.20.0 |
| [TypeScript](https://www.typescriptlang.org/docs/) | JavaScript 超集 | 6.0.3 |
| [pinia](https://pinia.vuejs.org/) | Vue 存储库替代 vuex5 | 3.0.4 |
| [vueuse](https://vueuse.org/) | 常用工具集 | 14.3.0 |
| [vue-i18n](https://kazupon.github.io/vue-i18n/zh/introduction.html/) | 国际化 | 11.4.4 |
| [vue-router](https://router.vuejs.org/) | Vue 路由 | 5.1.0 |
| [Tailwind CSS](https://tailwindcss.com/) | 原子 CSS | 4.3.0 |
| [Iconify](https://iconify.design/) | 图标组件 | 5.0.1 |
| [Iconify](https://icon-sets.iconify.design/) | 在线图标库 | 2.2.481 |
| [TinyMCE](https://www.tiny.cloud/) | 富文本编辑器 | 7.3.0 |
| [Echarts](https://echarts.apache.org/) | 图表库 | 6.1.0 |
| [axios](https://axios-http.com/) | http客户端 | 1.16.1 |
| [dayjs](https://day.js.org/) | 日期处理库 | 1.11.21 |
| [vee-validate](https://vee-validate.logaretm.com/) | 表单验证 | 4.15.1 |
| [zod](https://zod.dev/) | 数据验证 | 3.25.76 |

## 📖 使用场景

| 场景 | 说明 | 文档 |
|-----|------|------|
| 快速上手 | 了解 Skill 体系和使用方式 | [usage/quick-start.md](skills/usage/quick-start.md) |
| 新模块开发 | 参考模板创建新模块 Skill | [templates/skill-template.yaml](skills/templates/skill-template.yaml) |
| UI 框架适配 | TDesign 等 UI 框架开发规范 | [ui/frame/](skills/ui/frame/) |

## 📊 项目状态

- ✅ 3 个核心模块 Skill 已完成（core / effects / app-antd）
- ✅ Skill 模板和索引机制已就绪
- ✅ 上游代码已同步至 v2026.05

## 🔗 相关链接

- [yudao-ui-admin-vben](https://gitee.com/yudaocode/yudao-ui-admin-vben) - 上游前端仓库
- [yudao-skill-pro](https://github.com/cilangzzz/yudao-skill-pro) - 后端 Skill 知识库
- [芋道开发文档](https://doc.iocoder.cn/) - 芋道官方文档
- [vue-vben-admin](https://github.com/vbenjs/vue-vben-admin) - Vben Admin 上游

## 🐶 新手必读

- nodejs >= v22.18.0（推荐v24） && pnpm >= 11.0.0（强制使用 pnpm）
- 演示地址【Vue3 + element-plus】：<http://dashboard-vue3.yudao.iocoder.cn>
- 演示地址【Vue3 + vben5(ant-design-vue)】：<http://dashboard-vben.yudao.iocoder.cn>
- 演示地址【Vue2 + element-ui】：<http://dashboard.yudao.iocoder.cn>
- 启动文档：<https://doc.iocoder.cn/quick-start/>
- 视频教程：<https://doc.iocoder.cn/video/>

## 📄 开源协议

本项目基于 [MIT License](LICENSE) 开源。
