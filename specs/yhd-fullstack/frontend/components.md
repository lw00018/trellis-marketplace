# 组件规范

本文件覆盖 Vue SFC 结构、组件命名、注释、文件夹组织。

## SFC 固定结构

Vue 文件统一使用以下结构：

```vue
<template>
  <div class="page-wrapper-content">
    <!-- 内容 -->
  </div>
</template>

<script setup lang="ts">
import type { XxxItem } from '@/model/xxx'
import { xxxApiGetList } from '@/api/xxx.api'

defineOptions({ name: 'XxxList' })

// 搜索表单
const searchForm = reactive({ name: '', status: undefined })

// 加载状态
const loading = ref(false)

/**
 * @description: 获取列表数据
 */
const fetchList = async () => {
  loading.value = true
  try {
    await xxxApiGetList(searchForm)
  } finally {
    loading.value = false
  }
}

onMounted(() => fetchList())
</script>

<style lang="less" scoped>
.page-wrapper-content {
  padding: 16px;
}
</style>
```

## Script 内部顺序

1. `import`：外部依赖 → 内部模块 → 类型
2. 宏定义：`defineOptions`、`defineProps`、`defineEmits`
3. 常量 / 变量：`ref`、`reactive`、`computed`
4. 方法：业务处理函数、事件处理函数
5. 生命周期：`onMounted`、`watch` 等

## 组件命名

- 统一使用 `defineOptions({ name: 'PascalCase' })` 定义组件名。
- 页面组件 name 必须与路由 name 完全一致，`keepAlive` 依赖该一致性。
- 独立页面模式命名：`XxxList`、`XxxAdd`、`XxxEdit`。
- 抽屉 / 弹窗模式不需要新增 `XxxAdd` / `XxxEdit` 页面组件。
- 只读详情页不是默认脚手架页面；业务确需时使用 `XxxDetail`。
- 禁止在 `<script setup>` 标签上写 `name` 属性。
- 禁止同时使用 `script setup name` 和 `defineOptions` 双重命名。

## 注释与方法风格

详细注释规则见 [comments.md](./comments.md)。组件内遵循：

- 常量 / 变量：使用中文 `//` 行注释。
- 方法：使用 JSDoc，`@description:` 冒号必需。
- 方法统一使用 `const` 箭头函数。

```ts
// 搜索表单数据
const searchForm = reactive({ name: '', status: undefined })

// 加载状态
const loading = ref(false)

/**
 * @description: 处理删除
 * @param { string } id 记录 ID
 */
const handleDelete = (id: string) => {
  // ...
}
```

## 组件文件夹结构

组件目录参考 Ant Design Vue 的组件包思想：一个组件目录应自包含、职责清晰、只通过 `index.ts` 暴露对外能力；但业务项目不得照搬组件库的 `style/`、`demo/`、`__tests__/` 等发布型目录。

共享组件放在 `src/components/ComponentName/`，模块级组件放在 `src/views/[module]/components/ComponentName/`。基础结构如下：

```txt
ComponentName/
├── ComponentName.vue
├── index.ts
└── interface.ts
```

复杂组件允许扩展为：

```txt
ComponentName/
├── ComponentName.vue
├── index.ts
├── interface.ts
├── config.ts
├── context.ts
├── components/
└── composable/
```

文件职责：

- `ComponentName.vue`：组件主入口，只承载模板编排、props/emits 绑定和少量胶水逻辑。
- `index.ts`：组件唯一对外出口，负责导出默认组件和必要类型；外部不得绕过 `index.ts` 引入组件内部文件。
- `interface.ts`：组件 Props、Emits、Slots、Expose，以及当前组件目录内多文件共享的类型。
- `config.ts`：当前组件私有静态运行时配置，例如固定选项、状态映射、默认表单值、表格列配置。
- `context.ts`：当前组件与私有子组件之间的 provide / inject 上下文；没有跨层级状态共享时不得创建。
- `components/`：当前组件私有子组件，只服务当前组件包。
- `composable/`：当前组件私有响应式状态流程，例如 `useXxxForm`、`useXxxTable`、`useXxxDialog`。

追加判断：

- 模板中出现独立业务区域，或同一块 UI 在当前组件内复用时，追加 `components/`。
- 组件内出现可命名的状态流程时，追加 `composable/`。
- 私有子组件需要共享状态、注册实例、联动展开项或注入配置时，追加 `context.ts`。
- 组件 Props、Emits、Slots、Expose，以及当前组件目录内多文件共享的类型，统一放 `interface.ts`。
- 可被 API、Store、页面或多个组件复用的业务类型，必须上移到 `src/model/{moduleName}.ts`，不得留在组件目录。
- 组件私有的静态运行时数据放 `config.ts`；`config.ts` 只能导出不可变配置，不得包含 `ref`、`reactive`、`computed`、`watch`、生命周期、请求调用或事件流程。
- `composable/` 只负责封装响应式状态、派生状态、监听副作用、生命周期副作用，以及围绕这些状态暴露的操作函数。
- 不把 `composable/` 当作杂物目录；纯函数、静态配置、表格列、API 定义、类型定义不得为了拆文件而塞进 composable。
- 不为单个常量、单个方法或少量配置创建新文件；少量只在当前 `.vue` 使用的配置可以就近放在 `.vue` 中。

示例桶导出：

```ts
import ComponentName from './ComponentName.vue'
export type { ComponentNameProps, ComponentNameEmits } from './interface'
export default ComponentName
```

## 组件组织

- 项目级公共组件：`src/components/`，PascalCase 目录，自动导入。
- 模块级组件：`src/views/[module]/components/`。
- 新增和编辑表单默认提取为模块级公共组件。
- 单组件私有 UI 放组件目录内 `components/`；单组件私有的响应式状态流程放组件目录内 `composable/`。
- 组件对外类型和组件目录内共享类型放 `interface.ts`；组件私有静态运行时配置放 `config.ts`。
- 组件包内部跨层级注入能力放 `context.ts`；禁止把页面级或模块级全局状态塞进组件 `context.ts`。
- 模块内可复用的响应式状态流程放模块级 `composable/`；跨模块复用的组合式能力放 `src/hooks/`，纯工具函数放 `src/utils/`。
- Agent 生成或重构组件时，应在保持业务组件轻量的前提下借鉴组件包结构，主动拆分复杂组件，不需要等待用户逐项指定文件
