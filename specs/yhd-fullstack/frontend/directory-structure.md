# 目录结构规范

本文件覆盖项目整体骨架、业务模块脚手架落盘位置、自动加载目录、受保护文件。

**两种层级分开看**：

- **A. 项目全局骨架** — 预置目录，不由脚手架生成，已随项目初始化
- **B. 业务模块结构** — 新增一个业务模块（如 `order`、`user`）时，按固定位置落盘

---

## A. 项目全局骨架

```txt
项目根目录/
├── src/                          ← 源代码
│   ├── api/                        ← API 函数（{moduleName}.api.ts）
│   ├── assets/
│   │   ├── css/
│   │   │   └── variables.less      ← Less 变量（@primary-color 等）
│   │   ├── icon/                   ← Iconfont 资源
│   │   └── images/                 ← 图片资源
│   ├── components/                 ← 项目级公共组件（PascalCase 目录，自动导入）
│   ├── directives/                 ← 自定义指令（自动注册）
│   ├── enums/                      ← 枚举常量（cacheEnum、appEnum 等）
│   ├── exception/                  ← 异常页面（401、403、404）
│   ├── hooks/                      ← 通用 Hooks（useXxx）
│   │   ├── setting/                ← 系统设置 Hooks（useRootSetting）
│   │   └── web/                    ← 页面导航 Hooks（useGo、useTabs）
│   ├── layout/                     ← 布局组件（header / sidebar / tabs / content，受保护）
│   ├── mock/                       ← Mock 模拟数据（开发环境）
│   │   ├── _config.ts              ← Mock 全局配置
│   │   └── _util.ts                ← Mock 响应工具类
│   ├── model/                      ← 类型定义（接口 DTO/VO）
│   │   └── common.ts               ← MISSING_TYPE、ObjectExtends 等通用类型
│   ├── plugins/                    ← 插件（自动加载）
│   │   ├── index.ts                ← 插件自动加载入口
│   │   ├── antd.ts                 ← Ant Design Vue 按需注册
│   │   ├── components.ts           ← 项目公共组件注册
│   │   ├── custom.ts               ← Portal CMS 初始化（受保护）
│   │   └── modal.ts                ← 全局 Modal/message 配置
│   ├── request/                    ← HTTP 请求封装（基于 @yhdfe-plugins/axios，受保护）
│   │   └── index.ts
│   ├── router/
│   │   ├── index.ts                ← 路由聚合，自动读取 modules
│   │   ├── constant.ts             ← layout、empty、异常页、重定向常量
│   │   └── modules/                ← 路由模块（自动加载，业务路由落盘位置）
│   ├── store/                      ← Pinia Setup Store（{moduleName}.ts）
│   ├── types/                      ← src 内局部类型
│   ├── utils/                      ← 通用工具函数
│   │   └── cache.ts                ← 缓存工具（localCache、sessionCache）
│   ├── views/                      ← 页面视图
│   ├── App.vue                     ← 根组件（YConfigProvider + YAppVersionChecker）
│   └── main.ts                     ← 入口文件
├── tests/
│   └── unit/                       ← Jest 单元测试（**/*.spec.ts）
├── types/                          ← 根目录全局声明，自动生成文件也在这里
│   ├── app.config.ts
│   ├── env.config.ts
│   ├── auto-imports.d.ts           ← 自动生成，禁止手编
│   └── components.d.ts             ← 自动生成，禁止手编
├── app.config.ts                   ← 系统配置（受保护）
├── env.config.ts                   ← 环境配置（受保护）
├── api.json                        ← 代理配置来源
├── jest.config.js                  ← Jest 测试配置
├── babel.config.js                 ← Jest Babel 配置
├── tailwind.config.js              ← Tailwind 配置
└── vite.config.ts                  ← Vite 构建配置（受保护）
```

---

## B. 业务模块结构

新增业务模块 `xxx` 时，文件**必须**落到以下固定位置：

```txt
src/
├── api/
│   └── xxx.api.ts                  ← API 函数集合（函数名 xxxApiVerbObject）
├── mock/
│   └── xxx.ts                      ← Mock 数据
├── model/
│   └── xxx.ts                      ← 业务类型定义（XxxItem、XxxListParams、XxxCreateDTO 等）
├── router/
│   └── modules/
│       └── xxx.ts                  ← 路由模块（list 固定；add / edit 仅独立页模式）
├── store/
│   └── xxx.ts                      ← Pinia Setup Store（可选，按需）
└── views/
    └── xxx/                        ← 模块视图目录
        ├── list.vue                ← 列表页（XxxList）
        ├── add.vue                 ← 新增独立页（XxxAdd，按业务形态可选）
        ├── edit.vue                ← 编辑独立页（XxxEdit，按业务形态可选）
        ├── columns.ts              ← 表格列定义（列多时提取）
        ├── components/
        │   ├── XxxForm.vue         ← 模块级公共表单组件（新增与编辑共用）
        │   └── interface.ts        ← 组件 Props / Emits 类型
        └── composable/
            └── useXxx.ts           ← 模块级逻辑复用

tests/
└── unit/
    └── xxx.spec.ts                 ← Jest 单元测试，按实际复杂度继续拆分
```

### 独立页面模式命名

| 文件 | 组件 name（`defineOptions`） | 路由 name | path |
| --- | --- | --- | --- |
| `list.vue` | `XxxList` | `XxxList` | `/xxx/list` |
| `add.vue` | `XxxAdd` | `XxxAdd` | `/xxx/add` |
| `edit.vue` | `XxxEdit` | `XxxEdit` | `/xxx/edit/:id` |

**要点**：

- **没有默认** `detail.vue`（脚手架不生成；若业务确需只读详情页，单独申请并用 `XxxDetail`）
- `src/model/xxx.ts` 是业务类型唯一默认来源，详见 [type-safety.md](./type-safety.md)
- `list.vue` 负责查询与触发新增 / 编辑交互；交互形态可为路由跳转、抽屉或弹窗
- 独立页面模式使用 `add.vue` / `edit.vue`，抽屉 / 弹窗模式在 `list.vue` 内复用 `components/XxxForm.vue`
- 组件 `name` 必须与路由 `name` 完全一致（keepAlive 依赖）

### 测试目录组织

当前模板项目使用 Jest，`jest.config.js` 匹配 `tests/unit/**/*.spec.ts`。

- `tests/unit/xxx.spec.ts`
- `tests/unit/xxx/list.spec.ts`（复杂模块可继续按业务拆子目录）
- `tests/{module}/api/xxx.api.spec.ts`（不匹配当前 Jest 配置）
- `tests/api/xxx.api.spec.ts`（不要扁平散落）

如果项目后续改为 Vitest 或调整 Jest `testMatch`，必须先同步测试规范。

---

## 自动加载目录

| 目录 | 加载方式 |
| --- | --- |
| `src/router/modules/*.ts` | 自动注册路由 |
| `src/plugins/*.ts` | 自动加载插件 |
| `src/directives/*.ts` | 自动注册指令 |
| `src/components/*` | PascalCase 目录自动导入 |
| `src/mock/_config.ts` | Mock 全局配置 |
| `src/mock/_util.ts` | Mock 响应工具函数 |

---

## 受保护文件（禁止修改）

- `src/layout/`（布局，受保护）
- `src/request/index.ts`（HTTP 封装，受保护）
- `src/plugins/custom.ts`（Portal CMS 初始化，受保护）
- `src/plugins/index.ts`（插件自动加载入口）
- `src/plugins/antd.ts`（Ant Design Vue 全局注册清单，改动会影响全局组件可用性）
- `src/plugins/modal.ts`（全局 message/Modal 行为）
- `src/router/index.ts`、`src/router/constant.ts`（路由聚合与基础常量）
- `src/App.vue`、`src/main.ts`（应用入口）
- `app.config.ts`、`env.config.ts`、`api.json`（系统、环境与代理配置）
- `vite.config.ts`、`tsconfig.json`、`tailwind.config.js`、`postcss.config.js`（构建与编译配置）
- `.eslintrc-auto-import.json`、`types/auto-imports.d.ts`、`types/components.d.ts`（自动生成，禁止手编）
- `package.json`、`pnpm-lock.yaml`（依赖和脚本变更必须单独确认）

---

## 与后端对齐

- 前端模块名称应与后端领域模块保持一致。
- API 路径、错误码、枚举值必须来自共享契约或文档。
- 新增页面前先确认后端是否已有对应 DTO、VO 和分页结构。
