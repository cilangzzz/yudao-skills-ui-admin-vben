# Skill 快速上手指南

> 本文档提供快速使用 Skill 文档的提示词模板，帮助开发者快速定位代码、理解架构、完成开发任务。

---

## 一、Skill 快速使用指南

### 什么是 Skill 文档

Skill 文档是从 yudao-ui-admin-vben 项目代码中提取的结构化知识库，包含：
- **设计理念**：业务定位、设计原则、技术选型
- **架构设计**：目录结构、模块依赖、设计模式
- **核心实现**：类型定义、组件、函数、配置
- **代码规范**：组件使用、API调用、命名规范
- **扩展指南**：新增功能步骤、最佳实践

### 如何使用 Skill 文档

```
使用 Skill 文档，帮我 [任务描述]

参考 Skill 文档：
- skills/modules/[模块名]/skill-[模块名].yaml
```

**示例**：
```
使用 Skill 文档，帮我在 web-antd 应用添加一个"操作日志"页面。
参考 skills/modules/app-antd/skill-app-antd.yaml
```

---

## 二、一分钟提示词模板

### 基础模板

```
参考 Skill 文档 skills/modules/[模块]/skill-[模块].yaml，帮我完成以下任务：

[具体任务描述]

要求：
1. 遵循项目现有的代码规范
2. 使用统一的组件和API封装
3. 添加必要的权限控制
```

### 快速填充指南

| 场景 | 模块路径 | 模块名 |
|------|---------|--------|
| 核心组件开发 | packages/@core | core |
| 效应层功能 | packages/effects | effects |
| Ant Design应用 | apps/web-antd | app-antd |

---

## 三、常见场景快速提示词

### 3.1 查找代码位置

```
参考 Skill 文档，帮我找到 [功能] 相关的代码位置：
- 组件位置
- API接口
- 类型定义
- 配置文件

模块：[模块名]
功能关键词：[关键词]
```

**示例**：
```
参考 skills/modules/core/skill-core.yaml，帮我找到表单组件相关的代码位置：
- 表单组件位置
- 表单API
- 表单类型定义
```

---

### 3.2 理解架构设计

```
参考 Skill 文档 skills/modules/[模块]/skill-[模块].yaml，帮我理解 [架构场景] 的完整设计：

1. 模块职责是什么？
2. 与其他模块的依赖关系？
3. 核心组件有哪些？
4. 设计模式有哪些应用？
```

**示例**：
```
参考 skills/modules/core/skill-core.yaml，帮我理解表单系统设计：
1. 表单系统的核心抽象是什么？
2. 如何实现Schema配置？
3. 如何扩展新的表单组件？
```

---

### 3.3 新增业务页面

```
参考 Skill 文档 skills/modules/app-antd/skill-app-antd.yaml，帮我新增一个业务页面：

页面名称：[页面名]
业务模块：[模块名]
功能描述：[功能说明]
权限标识：[权限码]

请按照项目规范生成：
1. 路由配置
2. API接口
3. 页面组件
4. 表格/表单配置
```

**示例**：
```
参考 skills/modules/app-antd/skill-app-antd.yaml，帮我新增一个操作日志页面：

页面名称：操作日志
业务模块：system
功能描述：记录和查询用户操作日志
权限标识：system:operate-log:query

请按照项目规范生成完整代码。
```

---

### 3.4 新增API接口

```
参考 Skill 文档 skills/modules/app-antd/skill-app-antd.yaml，帮我新增API接口：

接口名称：[接口名]
接口路径：[HTTP方法和路径]
功能描述：[功能说明]
请求参数：[参数列表]
响应数据：[响应结构]

请按照项目规范生成API代码。
```

---

### 3.5 新增组件

```
参考 Skill 文档 skills/modules/core/skill-core.yaml，帮我新增一个组件：

组件名称：[组件名]
组件功能：[功能说明]
组件属性：[属性列表]
使用场景：[场景描述]

请按照项目规范生成：
1. 组件实现
2. 类型定义
3. 使用示例
```

---

## 四、Skill 文档快速导航

### 4.1 模块索引

| 模块 | 文档路径 | 核心功能 | 关键组件 |
|------|---------|---------|---------|
| **core** | skills/modules/core/skill-core.yaml | 基础设计、组合函数、UI组件抽象 | VbenForm, ModalApi, useNamespace |
| **effects** | skills/modules/effects/skill-effects.yaml | 权限、请求、Hooks、布局 | RequestClient, useAccess, BasicLayout |
| **app-antd** | skills/modules/app-antd/skill-app-antd.yaml | Ant Design应用实现 | Upload, TableAction, DictTag |

### 4.2 技术栈速查

| 分类 | 技术 | 版本 |
|------|------|------|
| 框架 | Vue | 3.5.30 |
| 构建 | Vite | 8.0.1 |
| 类型 | TypeScript | 5.9.3 |
| 状态 | Pinia | 3.0.4 |
| 路由 | Vue Router | 5.0.3 |
| 样式 | Tailwind CSS | 4.2.2 |
| UI | Ant Design Vue | 4.2.6 |

### 4.3 目录结构速记

```
apps/           → 应用层（web-antd, web-ele等）
packages/@core/ → 核心层（base, composables, ui-kit）
packages/effects/ → 效应层（access, request, hooks, layouts）
internal/       → 工具配置层（lint-configs, vite-config）
```

---

## 五、常用代码片段

### 5.1 表格页面

```vue
<script setup lang="ts">
import { useVbenVxeGrid } from '#/adapter/table';
import { XxxApi } from '#/api/module/xxx';

const [Grid, gridApi] = useVbenVxeGrid({
  columns: [
    { field: 'name', title: '名称' },
    { field: 'status', title: '状态' },
    { field: 'action', title: '操作', slots: { default: 'action' } }
  ],
  proxyConfig: {
    ajax: {
      query: async ({ page }) => {
        const res = await XxxApi.getList(page);
        return { items: res.list, total: res.total };
      }
    }
  }
});
</script>

<template>
  <Grid>
    <template #action="{ row }">
      <a-button type="link" @click="handleEdit(row)">编辑</a-button>
      <a-button type="link" danger @click="handleDelete(row)">删除</a-button>
    </template>
  </Grid>
</template>
```

### 5.2 表单弹窗

```vue
<script setup lang="ts">
import { useVbenForm, useVbenModal } from '@vben-core/form-ui';

const [BaseForm, formApi] = useVbenForm({
  schema: [
    { component: 'Input', fieldName: 'name', label: '名称', rules: 'required' },
    { component: 'Select', fieldName: 'status', label: '状态' }
  ]
});

const [Modal, modalApi] = useVbenModal({
  onConfirm: async () => {
    const values = await formApi.getValues();
    await XxxApi.create(values);
    modalApi.close();
  }
});
</script>

<template>
  <Modal title="新增">
    <BaseForm />
  </Modal>
</template>
```

### 5.3 API定义

```typescript
// api/module/xxx/index.ts
import { requestClient } from '#/api/request';

export namespace XxxApi {
  export const getList = (params: PageParams) =>
    requestClient.get('/module/xxx/page', { params });

  export const create = (data: XxxCreateDTO) =>
    requestClient.post('/module/xxx/create', data);

  export const update = (data: XxxUpdateDTO) =>
    requestClient.put('/module/xxx/update', data);

  export const delete = (id: number) =>
    requestClient.delete(`/module/xxx/delete?id=${id}`);
}
```

---

## 六、快速参考

### 6.1 命名规范

| 类型 | 命名规则 | 示例 |
|------|---------|------|
| 文件 | kebab-case | use-namespace.ts |
| 组件 | PascalCase | VbenForm |
| Hook | use前缀 | usePagination |
| 类型 | typing.ts / types.ts | types.ts |

### 6.2 开发命令

```bash
# 安装依赖
pnpm install

# 启动开发
pnpm dev:antd

# 构建生产
pnpm build:antd

# 代码检查
pnpm lint

# 类型检查
pnpm check:type
```

---

**提示**：使用 Skill 文档时，直接复制对应的提示词模板，替换占位符即可快速获得准确的代码生成和问题解答。