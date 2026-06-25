# 导入约束与自动导入

本文件覆盖 `unplugin-auto-import` / `unplugin-vue-components` 自动导入项，以及**强制**的导入来源约束。

## 自动导入（无需手动 import）

当前模板项目的自动导入以 `.eslintrc-auto-import.json` 和 `types/auto-imports.d.ts` 为准。常见无需手动导入项包括：

- **Vue API**：`ref`、`reactive`、`computed`、`watch`、`watchEffect`、`onMounted`、`onUnmounted`、`nextTick`、`toRef`、`toRefs`、`unref`、`defineComponent`、`defineAsyncComponent`、`inject`、`provide`、`useAttrs`、`useSlots` 等。
- **Portal hooks**：`useRoute`、`useRouter`、`usePortalCms`、`usePortalCmsUser`。
- **HTTP 请求实例**：`request` 自动指向 `src/request/index.ts` 的默认导出。
- **自动类型**：`RouteRecordRaw` 来自 `@yhdfe-plugins/portal-cms-vue`，`CustomResponse` 来自 `@yhdfe-plugins/axios`。

**质量检查**：编写完毕后移除所有可自动导入的显式 `import` 语句；如果自动导入声明缺失，不要手改生成文件，应调整生成配置后重新生成。

## 导入约束（强制）

| 导入项 | 正确来源 | 禁止来源 |
| --- | --- | --- |
| `RouteRecordRaw`、路由类型 | `@yhdfe-plugins/portal-cms-vue` | `vue-router` |
| `useRoute` / `useRouter` | `@yhdfe-plugins/portal-cms-vue` | `vue-router` |
| `usePortalCms` / `usePortalCmsUser` | `@yhdfe-plugins/portal-cms-vue` | 自行读取 token / user |
| `CustomResponse` 类型 | `@yhdfe-plugins/axios` | 自定义类型 |
| HTTP 请求 | `@/request`（封装 `@yhdfe-plugins/axios`） | 直接 `axios` |
| 根目录配置 | `~/app.config`、`~/env.config` | `@/../app.config` |
| lodash 函数 | `lodash-es`（按需导入） | `lodash` 全量导入 |
| dayjs | `dayjs` 直接导入 | `moment` |
| VueUse | `@vueuse/core`（按需导入） | — |
| 路径别名 | `@/xxx` (`src/xxx`)、`~/xxx`（项目根） | 相对路径 `../../` |

## 禁止手动编辑的生成文件

- `/types/auto-imports.d.ts`
- `/types/components.d.ts`

以上文件由构建插件自动生成，手动编辑会在下次构建时被覆盖。
