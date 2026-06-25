# 实现前检查清单

本清单用于在新增页面、组件、接口、Store、Hook、工具函数或配置前快速确认方向，减少重复实现和跨层遗漏。

## 1. 先搜索现有能力

开始写代码前先检索：

```bash
rg "页面关键词|接口路径|字段名|组件名|Hook名" src tests
```

- [ ] 是否已有相似页面、弹窗、表格、表单或详情组件？
- [ ] 是否已有 `src/api/` 接口封装？
- [ ] 是否已有 `src/model/` 类型？
- [ ] 是否已有 Store、Hook、Utils 或组件私有 composable 可复用？
- [ ] 是否已有 Ant Design Vue、YUI 或项目公共组件满足需求？

## 2. 判断新增位置

| 要新增的内容 | 推荐位置 |
| --- | --- |
| 页面 | `src/views/` 对应业务目录 |
| 路由 | `src/router/modules/` |
| API 方法 | `src/api/` |
| API 入参/响应类型 | `src/model/` |
| 跨页面公共组件 | `src/components/ComponentName/` |
| 当前组件私有子组件 | 当前组件目录 `components/` |
| 当前组件响应式流程 | 当前组件目录 `composable/` |
| 跨模块纯函数 | `src/utils/` |
| 跨页面响应式能力 | `src/hooks/` |
| 组件私有类型 | 组件目录 `interface.ts` |
| 组件私有静态配置 | 组件目录 `config.ts` |

不确定放哪里时，先查找同类实现，不要新建杂乱目录。

## 3. 类型与配置

- [ ] 是否需要新增 API model，而不是在页面内临时写类型？
- [ ] 枚举、状态映射、固定选项是否应放在 `config.ts`？
- [ ] Props、Emits、Slots、Expose 是否应放在 `interface.ts`？
- [ ] 表单默认值、表格列、筛选项是否需要和接口字段保持一致？
- [ ] 是否涉及 `undefined`、`null`、空字符串、空数组的语义差异？

## 4. 路由、权限与 Portal CMS

- [ ] 是否需要新增路由模块，而不是修改 `src/router/index.ts`？
- [ ] 是否使用 `@yhdfe-plugins/portal-cms-vue` 的 `useRoute`、`useRouter`、路由类型和 token 能力？
- [ ] 是否需要菜单权限、按钮权限、路由 meta 或标签页标题？
- [ ] 页面刷新、跳转、关闭标签页是否可复用 `useGo`、`useRedoPage`、`useTabs`？

禁止自行创建 Vue Router、HTTP Client 或 Portal CMS 实例。

## 5. 请求与状态

- [ ] 是否通过 `@/request` 发起请求？
- [ ] 是否需要 request 的 `{ loading: true }`？
- [ ] 是否需要 Pinia Store，还是页面局部状态足够？
- [ ] Store 状态是否需要缓存；如果需要，是否使用 `localCache` / `sessionCache`？
- [ ] 是否存在并发请求、重复提交、组件卸载后的状态更新风险？

## 6. UI 与文案

- [ ] 是否已有 Ant Design Vue 或 YUI 组件可复用？
- [ ] 表单校验、错误提示、空状态、loading 是否完整？
- [ ] 是否明确要求 i18n？若启用，页面标题、按钮、placeholder、表格列名和消息提示必须统一接入。
- [ ] 本地调试是否通过 hosts 虚拟域名访问，通常为 `dev.dgyiheda.com`？

## 7. 测试与验证

- [ ] 新增纯函数是否补 Jest 单元测试？
- [ ] 组件关键渲染、交互或条件分支是否需要 `@vue/test-utils` 测试？
- [ ] 测试文件是否放在 `tests/unit/**/*.spec.ts`？
- [ ] 修改字段、枚举或默认值时，是否同步更新测试断言？

## 8. 禁止快速绕路

- [ ] 不为单个业务需求修改 `src/main.ts`、`src/App.vue`、`src/plugins/index.ts`、`src/plugins/custom.ts`。
- [ ] 不直接使用 `localStorage` / `sessionStorage`。
- [ ] 不直接从 `vue-router` 导入路由 hooks 或类型。
- [ ] 不为了通过类型检查降低 `tsconfig`、ESLint 或测试配置严格度。
- [ ] 不新增与项目已有 Hooks、Utils、request、router、progress 重复的封装。
