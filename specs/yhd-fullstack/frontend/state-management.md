# Store 规范

本文件覆盖 Pinia Setup Store 风格与命名约定。

## 风格要求

**只使用 Setup Store 风格**（函数式组合 API），**禁止 Options Store**。

## Store 结构

```ts
import { defineStore } from 'pinia'

export const useXxxStore = defineStore('xxx', () => {
  // 状态 — 直接暴露 ref
  const list = ref<XxxItem[]>([])
  const loading = ref(false)

  // 计算属性 — 使用 computed()
  const getList = computed(() => list.value)
  const getLoading = computed(() => loading.value)

  /**
   * @description: 获取列表
   */
  const fetchList = async () => {
    loading.value = true
    try {
      const res = await xxxApiGetList({})
      list.value = res.data
    } finally {
      loading.value = false
    }
  }

  /**
   * @description: 重置状态
   */
  const reset = () => {
    list.value = []
    loading.value = false
  }

  return { list, loading, getList, getLoading, fetchList, reset }
})
```

## 命名约定

- **导出名称**：默认导出 camelCase store 函数（如 `appStore`、`userStore`、`multipleTabStore`）。
- **Store ID**：hyphenated 带 `app` 前缀（如 `'app'`、`'app-user'`、`'app-multipleTab'`）。
- **状态名称**：直接使用业务名（如 `pageLoading`、`user`、`tabList`）。
- **Getter 名称**：`get` 前缀（如 `getPageLoading`、`getMenuExpand`、`getTabList`）。
- **Action 名称**：setter 用 `set` 前缀，其他用描述性动词。
- **导入来源**：`Router`、`RouteRecordRaw`、`useRoute`、`useRouter` 等路由类型和 hooks 必须来自 `@yhdfe-plugins/portal-cms-vue`。

## Store 与全局 Hook

`src/hooks/setting/useRootSetting.ts` 是 `src/store/app.ts` 的对外封装。页面或组件读取布局、loading、菜单、Tabs 高度时优先使用 `useRootSetting()`，不要直接耦合 `appStore` 内部状态。

`src/hooks/web/usePage.ts`、`src/hooks/web/useTabs.ts` 封装页面跳转、刷新和标签页行为。涉及标签页关闭、刷新、标题修改时优先使用这些 Hooks。

## 硬性限制

- Store **必须**使用 Setup Store 风格，**禁止** Options Store。
- **禁止**直接使用 `localStorage` / `sessionStorage`，缓存**必须**使用 `localCache` / `sessionCache`。
- 禁止在业务 Store 中重复实现 `appStore`、`multipleTabStore`、`userStore` 已提供的全局状态能力。
- 禁止在 Store 中从 `vue-router` 导入路由类型或 hooks。
