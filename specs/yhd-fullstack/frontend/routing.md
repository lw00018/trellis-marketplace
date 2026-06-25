# 路由规范

本文件覆盖路由模块组织、命名规则、meta 字段。

## 路由模块位置

路由模块放在 `src/router/modules/` 下，`src/router/index.ts` 通过 `import.meta.glob('./modules/*.ts', { eager: true })` 自动聚合，无需手动注册。

`src/plugins/custom.ts` 使用 `createPortalCms` 创建门户插件和路由实例，业务代码不得绕过 portal 插件自行创建 Vue Router。

## 路由配置示例

以下示例为独立页面模式；抽屉 / 弹窗模式只保留列表路由，不生成新增 / 编辑路由。

```ts
import { layout } from '@/router/constant'
import type { RouteRecordRaw } from '@yhdfe-plugins/portal-cms-vue'

const routes: RouteRecordRaw[] = [
  {
    path: '/xxx',
    name: 'Xxx',
    component: layout,
    redirect: '/xxx/list',
    meta: { title: 'Xxx管理', icon: 'xxx-icon' },
    children: [
      {
        path: '/xxx/list',
        name: 'XxxList',
        component: () => import('@/views/xxx/list.vue'),
        meta: { title: 'Xxx列表', keepAlive: true }
      },
      {
        path: '/xxx/edit/:id',
        name: 'XxxEdit',
        component: () => import('@/views/xxx/edit.vue'),
        meta: { title: 'Xxx编辑', hideMenu: true }
      }
    ]
  }
]
export default routes
```

## 命名规则

- `path`：kebab-case（`/user-management`）
- `name`：PascalCase（`UserManagement`）
- **keepAlive 要求 name 与组件 `defineOptions.name` 完全一致**

## Meta 字段

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| title | `string` | 页面标题 |
| icon | `string` | 菜单图标 |
| keepAlive | `boolean` | 是否缓存 |
| affix | `boolean` | 是否固定标签 |
| hideMenu | `boolean` | 不显示在菜单 |
| hideTab | `boolean` | 不显示标签页 |
| hideBreadCrumbs | `boolean` | 隐藏面包屑 |
| full | `boolean` | 全屏页面 |
| cover | `boolean` | 覆盖式页面，标签页逻辑会跳过 |
| hideHeader | `boolean` | 隐藏头部 |
| realPath | `string` | 动态路由真实路径，用于标签页复用 |
| dynamicLevel | `number` | 动态路由最大打开数量 |

## 路由常量

- `layout`：默认布局，来自 `@/router/constant`。
- `empty`：二级菜单或中间层路由容器，来自 `@/router/constant`。
- `REDIRECT_NAME`：页面刷新重定向路由名，`useRedo` / `useRedoPage` 依赖它。
- 异常页路径由 `LOST_PAGE_PATH`、`NO_PERMISSION_PATH`、`NO_PERMISSION_PAGE_PATH` 管理。

## 硬性限制

- `RouteRecordRaw` **必须**从 `@yhdfe-plugins/portal-cms-vue` 使用 `import type` 导入，**禁止**从 `vue-router` 导入。
- `useRoute` / `useRouter` **必须**从 `@yhdfe-plugins/portal-cms-vue` 导入。
- 禁止在业务代码中调用 `createRouter`、`createWebHistory` 或自行维护路由实例。
- 禁止修改 `src/router/index.ts` 来注册单个业务路由；新增业务路由只放 `src/router/modules/*.ts`。
