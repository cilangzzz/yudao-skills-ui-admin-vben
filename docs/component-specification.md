# Vben Admin 组件设计与代码规范

## 1. 项目架构概述

### 1.1 组件分层架构

```
packages/
├── @core/                    # 核心层 - 框架无关的基础组件
│   ├── base/                 # 基础设施
│   │   ├── design/           # 设计系统
│   │   ├── icons/            # 图标库
│   │   ├── shared/           # 共享工具
│   │   └── typings/          # 类型定义
│   ├── composables/          # 组合式函数
│   ├── preferences/          # 偏好设置
│   └── ui-kit/               # UI 组件包
│       ├── form-ui/          # 表单组件
│       ├── layout-ui/        # 布局组件
│       ├── menu-ui/          # 菜单组件
│       ├── popup-ui/         # 弹窗组件
│       ├── shadcn-ui/        # 基础 UI 组件
│       └── tabs-ui/          # 标签页组件
│
├── effects/                  # 效果层 - 业务相关组件
│   ├── access/               # 权限控制
│   ├── common-ui/            # 通用业务组件
│   ├── hooks/                # 业务 Hooks
│   ├── layouts/              # 布局模板
│   ├── plugins/              # 插件
│   └── request/              # 请求封装
│
└── [其他包]/                 # 工具包
    ├── constants/            # 常量
    ├── locales/              # 国际化
    ├── stores/               # 状态管理
    ├── styles/               # 样式
    ├── types/                # 类型
    └── utils/                # 工具函数

apps/web-antd/src/
├── components/               # 业务组件
│   ├── cron-tab/             # Cron 表达式
│   ├── cropper/              # 图片裁剪
│   ├── description/          # 描述列表
│   ├── dict-tag/             # 字典标签
│   ├── form-create/          # 动态表单
│   ├── map/                  # 地图组件
│   ├── markdown-view/        # Markdown 预览
│   ├── operate-log/          # 操作日志
│   ├── shortcut-date-range-picker/  # 快捷日期选择
│   ├── table-action/         # 表格操作
│   ├── tinymce/              # 富文本编辑器
│   └── upload/               # 文件上传
```

## 2. 组件分类与职责

### 2.1 核心 UI 组件 (`packages/@core/ui-kit/shadcn-ui/`)

**职责**: 提供最基础的 UI 原子组件，不包含业务逻辑。

**核心组件**:
- `VbenButton` - 按钮
- `VbenCheckbox` - 复选框
- `VbenInput` - 输入框
- `VbenSelect` - 选择器
- `VbenTooltip` - 提示框
- `VbenLoading` - 加载
- `VbenAvatar` - 头像
- `VbenBreadcrumb` - 面包屑
- `VbenContextMenu` - 右键菜单

**设计原则**:
```typescript
// 组件 Props 定义规范
export interface VbenButtonProps {
  /** 渲染元素类型 */
  as?: AsTag | Component;
  /** 是否作为子元素渲染 */
  asChild?: boolean;
  /** 自定义类名 */
  class?: any;
  /** 是否禁用 */
  disabled?: boolean;
  /** 加载状态 */
  loading?: boolean;
  /** 尺寸 */
  size?: ButtonVariantSize;
  /** 变体样式 */
  variant?: ButtonVariants;
}
```

### 2.2 弹窗组件 (`packages/@core/ui-kit/popup-ui/`)

**职责**: 提供 Modal 和 Drawer 的状态管理机制。

**核心 API**:
```typescript
// ModalApi 类设计
class ModalApi {
  // 状态管理
  public store: Store<ModalState>;
  public sharedData: Record<'payload', any>;

  // 生命周期方法
  open(): void;
  close(): Promise<void>;
  lock(isLocked?: boolean): void;
  unlock(): void;

  // 数据传递
  setData<T>(payload: T): this;
  getData<T>(): T;

  // 状态更新
  setState(state: Partial<ModalState>): this;

  // 事件钩子
  onConfirm(): void;
  onCancel(): void;
  onOpened(): void;
  onClosed(): void;
}
```

**使用示例**:
```vue
<script setup lang="ts">
import { useVbenModal } from '@vben/common-ui';

const [Modal, modalApi] = useVbenModal({
  async onConfirm() {
    const { valid } = await formApi.validate();
    if (!valid) return;

    modalApi.lock();
    try {
      await saveData();
      await modalApi.close();
      emit('success');
    } finally {
      modalApi.unlock();
    }
  },
  async onOpenChange(isOpen: boolean) {
    if (isOpen) {
      const data = modalApi.getData<FormData>();
      await formApi.setValues(data);
    }
  },
});
</script>

<template>
  <Modal title="表单标题">
    <Form />
  </Modal>
</template>
```

### 2.3 表单组件 (`packages/@core/ui-kit/form-ui/`)

**职责**: 提供动态表单配置和验证能力。

**核心类型**:
```typescript
// 表单 Schema 定义
export interface FormSchema<T extends BaseFormComponentType = BaseFormComponentType> {
  /** 组件类型 */
  component: Component | T;
  /** 组件属性 */
  componentProps?: ComponentProps;
  /** 默认值 */
  defaultValue?: any;
  /** 字段依赖 */
  dependencies?: FormItemDependencies;
  /** 描述信息 */
  description?: CustomRenderType;
  /** 字段名 */
  fieldName: string;
  /** 帮助信息 */
  help?: CustomRenderType;
  /** 是否隐藏 */
  hide?: boolean;
  /** 标签 */
  label?: CustomRenderType;
  /** 自定义渲染 */
  renderComponentContent?: RenderComponentContentType;
  /** 校验规则 */
  rules?: FormSchemaRuleType;
  /** 后缀 */
  suffix?: CustomRenderType;
}

// 表单公共配置
export interface FormCommonConfig {
  colon?: boolean;
  componentProps?: ComponentProps;
  disabled?: boolean;
  formItemClass?: string | (() => string);
  hideLabel?: boolean;
  labelWidth?: number;
  wrapperClass?: string;
}
```

**使用示例**:
```vue
<script setup lang="ts">
import { useVbenForm } from '#/adapter/form';

const [Form, formApi] = useVbenForm({
  commonConfig: {
    componentProps: { class: 'w-full' },
    formItemClass: 'col-span-2',
    labelWidth: 120,
  },
  layout: 'horizontal',
  schema: [
    {
      component: 'Input',
      fieldName: 'name',
      label: '名称',
      rules: 'required',
    },
    {
      component: 'Select',
      fieldName: 'status',
      label: '状态',
      componentProps: {
        options: [
          { label: '启用', value: 1 },
          { label: '禁用', value: 0 },
        ],
      },
    },
  ],
  showDefaultActions: false,
});

// 表单操作
const values = await formApi.getValues();
await formApi.setValues({ name: 'test' });
const { valid } = await formApi.validate();
</script>
```

### 2.4 通用业务组件 (`packages/effects/common-ui/`)

**职责**: 提供业务通用的组合型组件。

**主要组件**:
| 组件 | 路径 | 说明 |
|------|------|------|
| `Page` | `components/page/` | 页面容器 |
| `EllipsisText` | `components/ellipsis-text/` | 文本省略 |
| `IconPicker` | `components/icon-picker/` | 图标选择器 |
| `JsonViewer` | `components/json-viewer/` | JSON 查看器 |
| `Captcha` | `components/captcha/` | 验证码 |
| `CountTo` | `components/count-to/` | 数字动画 |
| `ContentWrap` | `components/content-wrap/` | 内容包装 |

**Page 组件示例**:
```vue
<Page auto-content-height>
  <template #doc>
    <DocAlert title="文档标题" url="https://..." />
  </template>
  <template #title>页面标题</template>
  <template #description>页面描述</template>
  <template #extra>
    <Button>操作按钮</Button>
  </template>

  <!-- 主内容 -->
  <Grid />

  <template #footer>底部操作栏</template>
</Page>
```

### 2.5 业务组件 (`apps/web-antd/src/components/`)

**职责**: 项目特定的业务组件。

| 组件 | 说明 |
|------|------|
| `Upload` | 文件/图片上传 |
| `ImageUpload` | 图片上传 |
| `FileUpload` | 文件上传 |
| `TableAction` | 表格操作按钮组 |
| `DictTag` | 字典标签显示 |
| `Tinymce` | 富文本编辑器 |
| `CronTab` | Cron 表达式编辑 |
| `Cropper` | 图片裁剪 |
| `Map` | 地图选择 |
| `FormCreate` | 动态表单生成 |

## 3. 组件设计规范

### 3.1 组件文件结构

```
component-name/
├── index.ts           # 导出入口
├── component-name.vue # 主组件
├── typing.ts          # 类型定义
├── types.ts           # 类型定义（备选）
├── use-xxx.ts         # 组合式函数
├── helpers.ts         # 辅助函数
└── sub-component/     # 子组件目录
    └── sub.vue
```

### 3.2 组件命名规范

| 类型 | 命名规则 | 示例 |
|------|---------|------|
| 组件文件 | kebab-case | `image-upload.vue` |
| 组件名 | PascalCase | `ImageUpload` |
| Props 接口 | 组件名 + Props | `FileUploadProps` |
| Emits | kebab-case | `update:model-value` |
| Slots | kebab-case | `#toolbar-tools` |

### 3.3 组件定义模板

```vue
<script lang="ts" setup>
import type { PropType } from 'vue';

import { computed, ref, toRefs, watch } from 'vue';

// 1. 类型导入
import type { ComponentProps } from './typing';

// 2. 第三方依赖
import { someUtil } from '@vben/utils';

// 3. 组件导入
import { Button, Input } from 'ant-design-vue';

// 4. 定义组件选项
defineOptions({
  name: 'ComponentName',
  inheritAttrs: false,
});

// 5. 定义 Props
const props = withDefaults(defineProps<ComponentProps>(), {
  disabled: false,
  size: 'default',
});

// 6. 定义 Emits
const emit = defineEmits<{
  change: [value: string];
  'update:modelValue': [value: string];
}>();

// 7. 响应式状态
const { disabled, size } = toRefs(props);
const loading = ref(false);

// 8. 计算属性
const isDisabled = computed(() => props.disabled || loading.value);

// 9. 方法
function handleClick() {
  emit('change', 'value');
}

// 10. 生命周期与监听
watch(() => props.value, (val) => {
  // 处理值变化
});
</script>

<template>
  <div class="component-wrapper">
    <slot />
  </div>
</template>

<style scoped>
/* 组件样式 */
</style>
```

### 3.4 Props 定义规范

```typescript
// 使用 interface 定义 Props
export interface UploadProps {
  /** 接受的文件类型 */
  accept?: string[];
  /** 上传 API */
  api?: (file: File) => Promise<AxiosResponse>;
  /** 是否禁用 */
  disabled?: boolean;
  /** 最大文件数量 */
  maxNumber?: number;
  /** 最大文件大小 (MB) */
  maxSize?: number;
  /** v-model 绑定值 */
  modelValue?: string | string[];
}

// 使用 withDefaults 设置默认值
const props = withDefaults(defineProps<UploadProps>(), {
  accept: () => ['image/*'],
  disabled: false,
  maxNumber: 1,
  maxSize: 2,
});
```

### 3.5 双向绑定规范

```typescript
// 支持 v-model 和 value 两种方式
const props = defineProps<{
  modelValue?: string;
  value?: string;
}>();

const emit = defineEmits<{
  'update:modelValue': [value: string];
  'update:value': [value: string];
  change: [value: string];
}>();

// 计算当前绑定值
const currentValue = computed(() => {
  return props.modelValue === undefined ? props.value : props.modelValue;
});

// 更新值时同时触发两个事件
function updateValue(val: string) {
  emit('update:modelValue', val);
  emit('update:value', val);
  emit('change', val);
}
```

## 4. TypeScript 规范

### 4.1 类型定义

```typescript
// 使用 interface 定义对象类型
export interface UserInfo {
  id: number;
  name: string;
  email: string;
}

// 使用 type 定义联合类型、交叉类型
export type Status = 'active' | 'inactive';
export type UserWithStatus = UserInfo & { status: Status };

// 使用 enum 定义枚举
export enum UploadResultStatus {
  DONE = 'done',
  ERROR = 'error',
  UPLOADING = 'uploading',
}

// 使用泛型约束
export interface ApiResponse<T = any> {
  code: number;
  data: T;
  message: string;
}
```

### 4.2 类型导入

```typescript
// 类型导入使用 type 关键字
import type { PropType, Ref } from 'vue';
import type { UploadProps } from './typing';

// 运行时导入不加 type
import { ref, computed, watch } from 'vue';
```

### 4.3 路径别名

```json
// tsconfig.json
{
  "compilerOptions": {
    "paths": {
      "#/*": ["./src/*"],           // 应用路径别名
      "@vben/*": ["./packages/*"]   // 包路径别名
    }
  }
}
```

## 5. 代码风格指南

### 5.1 ESLint 配置要点

```javascript
// Vue 组件规范
{
  'vue/block-order': ['error', { order: ['script', 'template', 'style'] }],
  'vue/component-name-in-template-casing': ['error', 'PascalCase'],
  'vue/define-macros-order': ['error', {
    order: ['defineOptions', 'defineProps', 'defineEmits', 'defineSlots']
  }],
  'vue/require-default-prop': 'error',
  'vue/require-explicit-emits': 'error',
}

// TypeScript 规范
{
  '@typescript-eslint/consistent-type-definitions': 'off',
  '@typescript-eslint/no-explicit-any': 'off',
}
```

### 5.2 Vue 代码风格

```vue
<!-- 1. Script 在前，Template 在后，Style 在最后 -->
<script lang="ts" setup>
// 代码...
</script>

<template>
  <!-- 模板... -->
</template>

<style scoped>
/* 样式... */
</style>

<!-- 2. 属性使用 kebab-case -->
<Button
  type="primary"
  :loading="isLoading"
  @click="handleClick"
/>

<!-- 3. 自定义事件使用 kebab-case -->
<Component @update:model-value="handleUpdate" />

<!-- 4. 插槽使用 kebab-case -->
<template #toolbar-tools>
  <Button>操作</Button>
</template>

<!-- 5. 多行属性换行 -->
<Input
  v-model="value"
  placeholder="请输入"
  :disabled="isDisabled"
/>
```

### 5.3 CSS 规范

```vue
<style scoped>
/* 使用 scoped 限制样式作用域 */

/* 组件根类名 */
.component-name {
  /* 样式... */
}

/* 状态类名使用 is- 或 has- 前缀 */
.is-active { }
.has-error { }

/* 修改子组件样式使用 :deep() */
:deep(.ant-input) {
  border-radius: 4px;
}
</style>

<!-- 或使用 CSS Modules -->
<style module>
.ellipsisMultiLine {
  display: -webkit-box;
  -webkit-box-orient: vertical;
}
</style>

<template>
  <div :class="$style.ellipsisMultiLine">
    内容
  </div>
</template>
```

## 6. 最佳实践

### 6.1 组件通信模式

```
┌─────────────────────────────────────────────────────────────┐
│                      组件通信模式                            │
├─────────────────────────────────────────────────────────────┤
│  父 → 子: Props                                             │
│  子 → 父: Emits / defineExpose                              │
│  双向绑定: v-model (modelValue + update:modelValue)          │
│  跨层级: provide/inject                                     │
│  全局状态: Pinia Store                                      │
│  API 模式: 通过 ref 调用子组件方法                           │
└─────────────────────────────────────────────────────────────┘
```

### 6.2 组合式函数 (Composables)

```typescript
// hooks/use-upload.ts
export function useUpload(directory?: string) {
  const loading = ref(false);

  async function upload(file: File) {
    loading.value = true;
    try {
      const result = await uploadApi(file, directory);
      return result;
    } finally {
      loading.value = false;
    }
  }

  return {
    loading,
    upload,
  };
}

// 使用
const { loading, upload } = useUpload('images');
```

### 6.3 API 模式组件设计

```typescript
// 通过类封装组件 API
export class ModalApi {
  private store: Store<ModalState>;

  open() {
    this.store.setState({ isOpen: true });
  }

  close() {
    this.store.setState({ isOpen: false });
  }

  setData<T>(data: T) {
    this.sharedData.payload = data;
    return this;
  }
}

// 通过 Hook 创建实例
export function useVbenModal(options: ModalApiOptions = {}) {
  const modalApi = ref<ModalApi>();

  // 创建 API 实例
  modalApi.value = new ModalApi(options);

  // 连接组件
  const Modal = defineComponent({
    setup() {
      return () => h(ModalComponent, { modalApi: modalApi.value });
    },
  });

  return [Modal, modalApi.value] as const;
}
```

### 6.4 表格页面 CRUD 模板

```vue
<script lang="ts" setup>
import type { VxeTableGridOptions } from '#/adapter/vxe-table';

import { Page, useVbenModal } from '@vben/common-ui';
import { useVbenVxeGrid, TableAction, ACTION_ICON } from '#/adapter/vxe-table';

// 表单弹窗
const [FormModal, formModalApi] = useVbenModal({
  connectedComponent: Form,
  destroyOnClose: true,
});

// 表格
const [Grid, gridApi] = useVbenVxeGrid({
  formOptions: {
    schema: useGridFormSchema(),
  },
  gridOptions: {
    columns: useGridColumns(),
    proxyConfig: {
      ajax: {
        query: async ({ page }, formValues) => {
          return await getListApi({
            pageNo: page.currentPage,
            pageSize: page.pageSize,
            ...formValues,
          });
        },
      },
    },
  } as VxeTableGridOptions<ItemType>,
});

// 操作方法
function handleCreate() {
  formModalApi.setData(null).open();
}

function handleEdit(row: ItemType) {
  formModalApi.setData(row).open();
}

async function handleDelete(row: ItemType) {
  await deleteApi(row.id);
  gridApi.query();
}
</script>

<template>
  <Page auto-content-height>
    <FormModal @success="gridApi.query()" />
    <Grid table-title="数据列表">
      <template #toolbar-tools>
        <TableAction
          :actions="[{
            label: '新增',
            type: 'primary',
            icon: ACTION_ICON.ADD,
            onClick: handleCreate,
          }]"
        />
      </template>
      <template #actions="{ row }">
        <TableAction
          :actions="[
            { label: '编辑', onClick: handleEdit.bind(null, row) },
            { label: '删除', danger: true, popConfirm: { confirm: handleDelete.bind(null, row) } },
          ]"
        />
      </template>
    </Grid>
  </Page>
</template>
```

## 7. 目录结构示例

```
src/views/module/feature/
├── index.vue              # 列表页面
├── data.ts                # 表格列/表单配置
├── modules/
│   └── form.vue           # 表单弹窗
└── components/            # 页面专用组件
    └── custom-component.vue
```

## 8. 参考资源

- Vue 3 文档: https://vuejs.org/
- Vben Admin 文档: https://doc.vben.pro/
- Ant Design Vue: https://antdv.com/
- Tailwind CSS: https://tailwindcss.com/
- VeeValidate: https://vee-validate.logaretm.com/
- Zod: https://zod.dev/