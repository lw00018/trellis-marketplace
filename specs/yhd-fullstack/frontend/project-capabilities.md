# 项目内置能力与工具选型规范

本文件覆盖前端 Vue3 工程中已有 Hooks、Utils、第三方工具选型和 i18n 使用边界。新增页面、组件、Store、API 或通用能力前，应先确认本文件中的能力是否可复用。

## 选型顺序

1. 优先复用项目已有 Hooks、Utils、Store 和公共组件。
2. 响应式组合能力优先使用 VueUse：`@vueuse/core`。
3. VueUse 没有对应能力时，使用 `lodash-es` 按需导入。
4. 日期处理使用 `dayjs`。
5. 缓存使用 `localCache` / `sessionCache`。
6. 只有确实没有现成能力时，才新增 `utils`、`hooks` 或组件私有 `composable/`。

## 页面导航与刷新

项目导航能力位于 `src/hooks/web/usePage.ts`。

```ts
import { useGo, useRedo, useRedoPage } from '@/hooks/web/usePage'

const go = useGo()
const redo = useRedo()
const redoPage = useRedoPage()

go('/target-path')
go({ name: 'RouteName', params: { id: '1' } })
go('/path', true)
await redo()
await redoPage()
```

规则：

- 页面跳转优先使用 `useGo()`。
- 当前页组件刷新优先使用 `useRedo()` 或 `useRedoPage()`。
- Router hooks 和路由类型必须来自 `@yhdfe-plugins/portal-cms-vue`，不要从 `vue-router` 导入。
- 不要在页面中重复封装跳转、刷新或 redirect 逻辑。

## 标签页能力

项目标签页能力位于 `src/hooks/web/useTabs.ts`。

```ts
import { useTabs } from '@/hooks/web/useTabs'

const { close, closeAll, closeOther, setTitle } = useTabs()

await close()
await closeAll()
await closeOther()
await setTitle('新标题')
```

规则：

- 关闭当前页、关闭全部、关闭其他、修改标签页标题时优先使用 `useTabs()`。
- `useTabs()` 依赖 `app.config.ts` 中 `settingConfig.multiTabsConfig.enable`，未开启多标签时不要强行调用。
- 不要在页面里直接操作标签页 Store 的内部状态。

## 全局布局与页面状态

项目全局布局能力位于 `src/hooks/setting/useRootSetting.ts`。

```ts
import { useRootSetting } from '@/hooks/setting/useRootSetting'

const {
  getPageLoading,
  getOpenPageLoading,
  getStyles,
  getHeaderHeight,
  getMenuWidth,
  getMenuMaxWidth,
  getTabsHeight,
  getContainerRef,
  setContainerRef,
  setPageLoading,
  setOpenPageLoading,
  setMenuWidth,
  getBaseHomePath,
  setMenuExpand,
  getMenuExpand,
} = useRootSetting()
```

规则：

- 页面或组件读取布局、loading、菜单宽度、Tabs 高度时优先使用 `useRootSetting()`。
- 不要直接耦合 `src/store/app.ts` 的内部状态。
- 不要为单个页面改 `src/App.vue` 或应用入口来处理布局状态。

## 缓存能力

缓存工具位于 `src/utils/cache.ts`。

```ts
import { localCache, sessionCache } from '@/utils/cache'

localCache.setItem('key', value)
const value = localCache.getItem('key')
localCache.removeItem('key')
localCache.clear()

sessionCache.setItem('key', value)
```

规则：

- 禁止直接使用 `localStorage` / `sessionStorage`。
- Store 中缓存标签页、布局或业务状态时使用 `localCache` / `sessionCache`。
- 缓存对象会自动 `JSON.stringify`，读取时会尝试 `JSON.parse`。
- 缓存 key、默认值和清理时机必须集中管理，不要散落在多个页面。

## 路由工具

路由工具位于 `src/utils/router.ts`。

```ts
import { flattenRouter, hasMenuShow, hasPermissionShow } from '@/utils/router'
```

- `flattenRouter(nestedRouter)`：扁平化嵌套路由。
- `hasMenuShow(route)`：判断侧边栏是否展示。
- `hasPermissionShow(route)`：判断路由权限是否可展示。

规则：

- 路由类型必须来自 `@yhdfe-plugins/portal-cms-vue`。
- 不要新增重复的路由扁平化、菜单展示或权限展示工具。
- 不要修改 `src/router/index.ts` 来注册单个业务路由。

## 进度条、浏览器检测与复制过滤

### 进度条

进度条工具位于 `src/utils/progress.ts`。

```ts
import progress from '@/utils/progress'

progress.startLoading()
progress.endLoading()
progress.resetLoading()
```

`src/request/index.ts` 已经根据请求配置中的 `loading` 自动调用进度条。业务 API 一般只需要传：

```ts
request.get('/xxx', { loading: true })
```

不要在业务页面中围绕同一个请求重复手动调用 `progress`。

### 浏览器检测

`src/main.ts` 启动前会执行 `browserDetection()`，不兼容时跳转 `/recommend.html`。业务代码不要重复做全局浏览器兼容检测。

### 复制过滤

`src/App.vue` 已统一监听 copy 事件并执行 `copyFilterSpaces`。业务页面不要重复注册同类全局 copy 监听。

## 新增能力放置规则

| 能力类型 | 推荐位置 |
| --- | --- |
| 通用纯函数 | `src/utils/` |
| 跨页面响应式能力 | `src/hooks/` |
| 单个复杂组件的响应式流程 | 组件目录 `composable/` |
| 组件私有类型 | 组件目录 `interface.ts` |
| 组件私有静态配置 | 组件目录 `config.ts` |
| 跨模块业务类型 | `src/model/` |
| API 方法 | `src/api/` |

新增通用纯函数或复杂状态流程时，应补充 Jest 单元测试。

## 国际化规则

- 明确要求国际化时才启用 `i18n`，否则禁止使用。
- 启用后，页面标题、按钮文案、表单标签、placeholder、表格列名、消息提示等用户可见文案必须统一接入 i18n。
- 禁止只对部分按钮或提示语做国际化。
- 不启用 i18n 的页面，应保持硬编码中文文案风格一致，不要混用局部翻译 key。

## 禁止项

- 禁止直接读取或写入浏览器 Storage。
- 禁止使用 `lodash` 全量导入。
- 禁止使用 `moment`。
- 禁止新增重复的缓存、路由、进度条、浏览器检测、复制过滤工具。
- 禁止绕过 Portal CMS 自行创建 Router、HTTP Client、token 或用户体系。
- 禁止把项目已有 Hooks 和 Utils 的包装函数散落在页面中。
