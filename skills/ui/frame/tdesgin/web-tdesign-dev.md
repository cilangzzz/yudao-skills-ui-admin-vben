---
name: web-tdesign-dev
description: web-tdesign 应用页面开发技能，提供 CRUD 页面开发完整指南，包含表格、表单、弹窗、API 调用等核心功能的使用方法和代码模板。
---

# web-tdesign 页面开发指南

本技能提供 web-tdesign 应用的页面开发指南，涵盖标准 CRUD 页面开发、组件使用、API 调用和编码规范。

## 触发条件

当用户需要执行以下任务时使用此技能：
- 在 web-tdesign 应用中创建新的业务页面
- 开发标准的 CRUD（增删改查）功能模块
- 使用 TDesign 组件库进行表单、表格开发
- 理解 web-tdesign 应用的代码规范和最佳实践
- 排查页面开发中的问题

## 源码与参考位置

| 资源 | 路径 |
|------|------|
| **表格适配器** | `src/adapter/vxe-table.ts` |
| **表单适配器** | `src/adapter/form.ts` |
| **组件类型定义** | `src/adapter/component/index.ts` |
| **请求客户端** | `src/api/request.ts` |
| **完整页面示例** | `src/views/system/user/` |
| **API 示例** | `src/api/system/user/index.ts` |

## 1. 项目概述

### 1.1 技术栈

| 层级 | 技术 | 说明 |
|------|------|------|
| 框架 | Vue 3 + TypeScript | 前端框架 |
| 构建 | Vite | 开发与构建 |
| UI库 | TDesign Vue Next | 腾讯设计体系 |
| 表格 | VxeTable + @vben/plugins/vxe-table | 高性能表格 |
| 表单 | @vben/common-ui (VbenForm) | 动态表单 |
| 状态 | Pinia | 状态管理 |
| HTTP | Axios + RequestClient | 请求封装 |

### 1.2 目录结构

```
apps/web-tdesign/src/
├── adapter/              # 适配器层
│   ├── form.ts          # 表单适配，导出 useVbenForm
│   ├── vxe-table.ts     # 表格适配，导出 useVbenVxeGrid
│   └── component/       # 组件类型定义
├── api/                  # API 接口层
│   ├── request.ts       # 请求客户端
│   └── system/          # 系统模块 API
├── components/           # 业务组件
│   ├── dict-tag/        # 字典标签
│   └── table-action/    # 表格操作列
└── views/                # 页面视图
    └── system/user/     # 用户管理示例
        ├── index.vue    # 主页面
        ├── data.ts      # 表单/表格配置
        └── modules/     # 子模块
            └── form.vue # 表单弹窗
```

## 2. 快速开始

### 2.1 创建新页面步骤

```bash
# 1. 创建页面目录
src/views/mymodule/myfeature/
├── index.vue      # 主页面
├── data.ts        # 表单 Schema、表格列配置
└── modules/
    └── form.vue   # 新增/编辑表单弹窗

# 2. 创建 API 文件
src/api/mymodule/myfeature/index.ts

# 3. 配置路由（如需静态路由）
src/router/routes/modules/mymodule.ts
```

### 2.2 标准页面结构

```
views/mymodule/myfeature/
├── index.vue            # 主页面（表格、搜索、操作）
├── data.ts              # 表单Schema、表格列配置
└── modules/             # 子模块组件
    ├── form.vue         # 新增/编辑表单弹窗
    └── ...其他组件
```

## 3. 核心 API/Hooks

### 3.1 useVbenVxeGrid - 表格

**导入路径**: `#/adapter/vxe-table`

```typescript
import { useVbenVxeGrid, TableAction, ACTION_ICON } from '#/adapter/vxe-table';

const [Grid, gridApi] = useVbenVxeGrid({
  formOptions: {
    schema: useGridFormSchema(),  // 搜索表单 Schema
  },
  gridOptions: {
    columns: useGridColumns(),    // 表格列配置
    proxyConfig: {
      ajax: {
        query: async ({ page }, formValues) => {
          return await getXxxPage({
            pageNo: page.currentPage,
            pageSize: page.pageSize,
            ...formValues,
          });
        },
      },
    },
  },
});

// 刷新表格
gridApi.query();
```

**内置单元格渲染器**:

| 名称 | 用途 | 示例 |
|------|------|------|
| CellImage | 单图显示 | `{ name: 'CellImage' }` |
| CellImages | 多图显示 | `{ name: 'CellImages' }` |
| CellLink | 链接按钮 | `{ name: 'CellLink', props: { text: '详情' } }` |
| CellTag | 标签 | `{ name: 'CellTag', props: { color: 'success' } }` |
| CellDict | 字典标签 | `{ name: 'CellDict', props: { type: 'dict_type' } }` |
| CellSwitch | 开关 | `{ name: 'CellSwitch', attrs: { beforeChange } }` |

### 3.2 useVbenModal - 弹窗

**导入路径**: `@vben/common-ui`

```typescript
import { useVbenModal } from '@vben/common-ui';

const [Modal, modalApi] = useVbenModal({
  connectedComponent: Form,  // 连接的组件
  destroyOnClose: true,      // 关闭时销毁
});

// 打开弹窗（可传递数据）
formModalApi.setData(row).open();

// 关闭弹窗
modalApi.close();
```

### 3.3 useVbenForm - 表单

**导入路径**: `#/adapter/form`

```typescript
import { useVbenForm, z } from '#/adapter/form';
import type { VbenFormSchema } from '#/adapter/form';

const [Form, formApi] = useVbenForm({
  commonConfig: {
    componentProps: { class: 'w-full' },
    labelWidth: 80,
  },
  layout: 'horizontal',
  schema: useFormSchema(),
});

// 表单操作
await formApi.validate();           // 验证
const values = await formApi.getValues();  // 获取值
await formApi.setValues(data);      // 设置值
```

**支持的组件类型**:

| 组件名 | 说明 |
|--------|------|
| Input | 输入框 |
| InputNumber | 数字输入框 |
| Textarea | 文本域 |
| Select | 下拉选择 |
| ApiSelect | 远程下拉选择 |
| TreeSelect | 树形选择 |
| ApiTreeSelect | 远程树形选择 |
| RadioGroup | 单选按钮组 |
| CheckboxGroup | 复选框组 |
| Switch | 开关 |
| DatePicker | 日期选择 |
| RangePicker | 日期范围选择 |
| Upload | 上传 |
| IconPicker | 图标选择 |

### 3.4 requestClient - API 调用

**导入路径**: `#/api/request`

```typescript
import { requestClient } from '#/api/request';

// 基础调用
requestClient.get('/api/xxx', { params });
requestClient.post('/api/xxx', data);
requestClient.put('/api/xxx', data);
requestClient.delete('/api/xxx');

// 分页查询
requestClient.get<PageResult<XxxItem>>('/api/xxx/page', { params });
```

**API 文件标准模式**:

```typescript
// api/module/xxx/index.ts
import { requestClient } from '#/api/request';

export namespace XxxApi {
  export interface Item {
    id?: number;
    name: string;
    status: number;
  }
}

export function getXxxPage(params: PageParam) {
  return requestClient.get<PageResult<XxxApi.Item>>('/module/xxx/page', { params });
}

export function createXxx(data: XxxApi.Item) {
  return requestClient.post('/module/xxx/create', data);
}

export function deleteXxx(id: number) {
  return requestClient.delete(`/module/xxx/delete?id=${id}`);
}
```

## 4. 标准 CRUD 页面模板

### 4.1 主页面 (index.vue)

```vue
<script lang="ts" setup>
import type { VxeTableGridOptions } from '#/adapter/vxe-table';
import type { XxxApi } from '#/api/module/xxx';

import { ref } from 'vue';
import { confirm, Page, useVbenModal } from '@vben/common-ui';
import { isEmpty } from '@vben/utils';

import { message } from '#/adapter/tdesign';
import { ACTION_ICON, TableAction, useVbenVxeGrid } from '#/adapter/vxe-table';
import { deleteXxx, getXxxPage } from '#/api/module/xxx';
import { $t } from '#/locales';

import { useGridColumns, useGridFormSchema } from './data';
import Form from './modules/form.vue';

const [FormModal, formModalApi] = useVbenModal({
  connectedComponent: Form,
  destroyOnClose: true,
});

function handleRefresh() {
  gridApi.query();
}

function handleCreate() {
  formModalApi.setData(null).open();
}

function handleEdit(row: XxxApi.Item) {
  formModalApi.setData(row).open();
}

async function handleDelete(row: XxxApi.Item) {
  await deleteXxx(row.id!);
  message.success($t('ui.actionMessage.deleteSuccess'));
  handleRefresh();
}

const [Grid, gridApi] = useVbenVxeGrid({
  formOptions: { schema: useGridFormSchema() },
  gridOptions: {
    columns: useGridColumns(),
    proxyConfig: {
      ajax: {
        query: async ({ page }, formValues) => {
          return await getXxxPage({
            pageNo: page.currentPage,
            pageSize: page.pageSize,
            ...formValues,
          });
        },
      },
    },
  } as VxeTableGridOptions<XxxApi.Item>,
});
</script>

<template>
  <Page auto-content-height>
    <FormModal @success="handleRefresh" />
    <Grid table-title="列表">
      <template #toolbar-tools>
        <TableAction
          :actions="[
            {
              label: $t('ui.actionTitle.create', ['xxx']),
              type: 'primary',
              icon: ACTION_ICON.ADD,
              auth: ['module:xxx:create'],
              onClick: handleCreate,
            },
          ]"
        />
      </template>
      <template #actions="{ row }">
        <TableAction
          :actions="[
            {
              label: $t('common.edit'),
              variant: 'text',
              icon: ACTION_ICON.EDIT,
              onClick: handleEdit.bind(null, row),
            },
            {
              label: $t('common.delete'),
              variant: 'text',
              type: 'danger',
              icon: ACTION_ICON.DELETE,
              popConfirm: {
                title: $t('ui.actionMessage.deleteConfirm', [row.name]),
                confirm: handleDelete.bind(null, row),
              },
            },
          ]"
        />
      </template>
    </Grid>
  </Page>
</template>
```

### 4.2 数据配置 (data.ts)

```typescript
import type { VbenFormSchema } from '#/adapter/form';
import type { VxeTableGridOptions } from '#/adapter/vxe-table';

import { DICT_TYPE } from '@vben/constants';
import { getDictOptions } from '@vben/hooks';
import { z } from '#/adapter/form';

/** 搜索表单 */
export function useGridFormSchema(): VbenFormSchema[] {
  return [
    {
      fieldName: 'name',
      label: '名称',
      component: 'Input',
      componentProps: { placeholder: '请输入名称', allowClear: true },
    },
    {
      fieldName: 'status',
      label: '状态',
      component: 'Select',
      componentProps: {
        options: getDictOptions(DICT_TYPE.COMMON_STATUS, 'number'),
        placeholder: '请选择状态',
        allowClear: true,
      },
    },
  ];
}

/** 新增/编辑表单 */
export function useFormSchema(): VbenFormSchema[] {
  return [
    {
      component: 'Input',
      fieldName: 'id',
      dependencies: { triggerFields: [''], show: () => false },
    },
    {
      fieldName: 'name',
      label: '名称',
      component: 'Input',
      rules: 'required',
    },
    {
      fieldName: 'status',
      label: '状态',
      component: 'RadioGroup',
      componentProps: {
        options: getDictOptions(DICT_TYPE.COMMON_STATUS, 'number'),
        buttonStyle: 'solid',
        optionType: 'button',
      },
      rules: z.number().default(1),
    },
  ];
}

/** 表格列 */
export function useGridColumns(): VxeTableGridOptions['columns'] {
  return [
    { type: 'checkbox', width: 40 },
    { field: 'id', title: '编号', minWidth: 100 },
    { field: 'name', title: '名称', minWidth: 120 },
    {
      field: 'status',
      title: '状态',
      minWidth: 100,
      cellRender: {
        name: 'CellDict',
        props: { type: DICT_TYPE.COMMON_STATUS },
      },
    },
    {
      field: 'createTime',
      title: '创建时间',
      minWidth: 180,
      formatter: 'formatDateTime',
    },
    { title: '操作', width: 160, fixed: 'right', slots: { default: 'actions' } },
  ];
}
```

### 4.3 表单弹窗 (modules/form.vue)

```vue
<script lang="ts" setup>
import type { XxxApi } from '#/api/module/xxx';

import { computed, ref } from 'vue';
import { useVbenModal } from '@vben/common-ui';
import { useVbenForm } from '#/adapter/form';
import { message } from '#/adapter/tdesign';
import { createXxx, getXxx, updateXxx } from '#/api/module/xxx';
import { $t } from '#/locales';

import { useFormSchema } from '../data';

const emit = defineEmits(['success']);
const formData = ref<XxxApi.Item>();
const getTitle = computed(() =>
  formData.value?.id
    ? $t('ui.actionTitle.edit', ['xxx'])
    : $t('ui.actionTitle.create', ['xxx'])
);

const [Form, formApi] = useVbenForm({
  commonConfig: { componentProps: { class: 'w-full' }, labelWidth: 80 },
  layout: 'horizontal',
  schema: useFormSchema(),
  showDefaultActions: false,
});

const [Modal, modalApi] = useVbenModal({
  async onConfirm() {
    const { valid } = await formApi.validate();
    if (!valid) return;
    modalApi.lock();
    const data = await formApi.getValues();
    try {
      await (formData.value?.id ? updateXxx(data) : createXxx(data));
      await modalApi.close();
      emit('success');
      message.success($t('ui.actionMessage.operationSuccess'));
    } finally {
      modalApi.unlock();
    }
  },
  async onOpenChange(isOpen: boolean) {
    if (!isOpen) {
      formData.value = undefined;
      return;
    }
    const data = modalApi.getData<XxxApi.Item>();
    if (data?.id) {
      modalApi.lock();
      try {
        formData.value = await getXxx(data.id);
        await formApi.setValues(formData.value);
      } finally {
        modalApi.unlock();
      }
    }
  },
});
</script>

<template>
  <Modal :title="getTitle" class="w-1/3">
    <Form class="mx-4" />
  </Modal>
</template>
```

## 5. 编码规范

### 5.1 推荐做法

- ✅ **使用 namespace 组织 API 类型**: `export namespace XxxApi { export interface Item {...} }`
- ✅ **从适配器导入 Hooks**: `import { useVbenForm } from '#/adapter/form'`
- ✅ **表单验证使用 rules 字符串**: `rules: 'required'` 或 `rules: z.string().min(5)`
- ✅ **字典使用 DICT_TYPE 常量**: `getDictOptions(DICT_TYPE.COMMON_STATUS, 'number')`
- ✅ **表格列使用 columns 配置**: 而非 template 中写 vxe-column

### 5.2 反模式

- ❌ 不要直接使用 TDesign 的 Form 组件，使用 VbenForm
- ❌ 不要在模板中定义表格列，使用 columns 配置
- ❌ 不要重复定义类型，从 API 文件导入
- ❌ 不要硬编码字典值，使用 DICT_TYPE 常量

## 6. 常用工具函数

```typescript
// 字典
import { getDictOptions, getDictLabel } from '@vben/hooks';
import { DICT_TYPE } from '@vben/constants';

// 获取字典选项（返回数字类型值）
getDictOptions(DICT_TYPE.COMMON_STATUS, 'number');

// 工具函数
import { isEmpty, downloadFileFromBlobPart, handleTree } from '@vben/utils';

// 消息提示
import { message, dialog, notification } from '#/adapter/tdesign';
message.success('操作成功');

// 确认对话框
import { confirm } from '@vben/common-ui';
await confirm('确定要删除吗？');
```

## 7. 关键文件路径

| 文件 | 说明 |
|------|------|
| `src/adapter/vxe-table.ts` | 表格适配器，包含 CellSwitch、CellDict 等渲染器 |
| `src/adapter/form.ts` | 表单适配器，定义 ComponentType |
| `src/adapter/component/index.ts` | 组件类型定义 |
| `src/views/system/user/index.vue` | 完整 CRUD 页面示例 |
| `src/views/system/user/data.ts` | 表单 Schema 和表格列配置示例 |
| `src/api/system/user/index.ts` | API 定义示例 |