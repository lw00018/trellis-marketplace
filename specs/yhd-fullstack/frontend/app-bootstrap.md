# 应用入口与插件规范

本文件覆盖前端 Vue3 工程的应用启动、插件加载、Portal CMS、Ant Design Vue 和 YUI 初始化约定。

## main.ts 启动顺序

`src/main.ts` 是应用入口，启动顺序固定：

1. `createApp(App)` 创建应用。
2. `loadAllPlugins(app)` 加载 `src/plugins` 下插件。
3. `loadAllDirectives(app)` 加载自定义指令。
4. `app.use(createPinia())` 加载 Pinia。
5. `app.mount('#app')` 挂载应用。

入口全局资源包括：

- `tailwindcss/tailwind.css`
- `normalize.css`
- `@/assets/icon/iconfont.css`

启动前会执行 `browserDetection()`，不兼容时跳转 `/recommend.html`。

## 本地服务访问

本地开发服务一般不可直接通过 `localhost`、`127.0.0.1` 或内网域名访问，应通过 hosts 配置的虚拟域名访问，通常为 `dev.dgyiheda.com`。

需要在浏览器打开页面、配置回调地址、调试登录态、Cookie、Portal CMS 或跨域请求时，优先使用该虚拟域名，避免因域名不一致导致权限、Cookie、接口代理或环境识别异常。

## 插件自动加载

`src/plugins/index.ts` 使用 `import.meta.glob('./**/*.ts', { eager: true })` 自动加载插件：

- 默认导出必须是函数。
- 以 `_` 开头的文件不会加载。
- `index.ts` 不会加载自身。

新增全局插件时放在 `src/plugins/*.ts`，默认导出 `(app: App) => void` 函数。不要修改 `src/plugins/index.ts` 注册单个插件。

## Portal CMS

`src/plugins/custom.ts` 使用 `createPortalCms` 统一创建门户插件、路由、HTTP、权限和登录态。

业务代码必须通过 `@yhdfe-plugins/portal-cms-vue` 获取：

- `useRoute`
- `useRouter`
- `usePortalCms`
- `usePortalCmsUser`
- `getToken`
- `RouteRecordRaw` 等路由类型

禁止自行创建 Vue Router、绕过 Portal CMS 读取 token、重复实现登录态或权限体系。

## App.vue 全局容器

`src/App.vue` 承载全局容器能力：

- Ant Design Vue `ConfigProvider` 和中文 locale。
- YUI `YConfigProvider`。
- YUI `YAppVersionChecker`。
- `useRootSetting()` 页面 loading。
- 监听路由变化更新 YUI `setting.path`。
- copy 事件过滤。

业务需求不得随意改动 `App.vue`。页面级逻辑应放到具体页面、组件、Store 或 Hook 中。

## Ant Design Vue 注册

`src/plugins/antd.ts` 按需全局注册 Ant Design Vue 组件。模板项目已注册常用组件，例如：

- Form / Row / Col / Select / Input / Button
- Table / Tag / Switch / Steps / Divider
- Upload / Image / Drawer / Dropdown / Menu / Modal
- Popover / Checkbox / Tree / Pagination

使用未注册的 Ant Design Vue 组件时，优先确认项目自动组件声明和 `src/plugins/antd.ts`，不要在多个业务组件中重复局部注册同一个全局基础组件。

## 项目公共组件注册

`src/plugins/components.ts` 注册项目公共组件，例如 `Icon`、`Empty`。

项目级公共组件放在 `src/components/ComponentName/`，通过 `index.ts` 暴露；需要全局注册时在 `components.ts` 中统一注册。

## 全局反馈配置

`src/plugins/modal.ts` 配置全局 `message` 展示时间和 `Modal.dangerWarning`。

删除、危险操作和全局确认弹窗应优先使用项目已有封装，不要在页面中重复拼接危险弹窗样式。

## 禁止项

- 禁止为单个业务需求随意修改 `src/main.ts`、`src/App.vue`、`src/plugins/index.ts`、`src/plugins/custom.ts`。
- 禁止绕过 `src/plugins/antd.ts` 在多个业务组件中重复注册全局基础组件。
- 禁止自行创建 Router、HTTP Client、Portal CMS 实例。
- 禁止把页面业务逻辑塞进应用入口或全局插件。
