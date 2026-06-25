# 前端开发指南索引

> **定位**：服务于 Java 全栈项目的前端协作规范，重点保证 API 契约、认证状态、错误处理和类型边界一致。

## 文档文件

| 文件 | 说明 | 何时阅读 |
| --- | --- | --- |
| [directory-structure.md](./directory-structure.md) | 项目骨架、业务模块落盘位置、自动加载目录、受保护文件 | 新建页面或模块时 |
| [app-bootstrap.md](./app-bootstrap.md) | 应用入口、插件加载、Portal CMS、Ant Design Vue 和 YUI 初始化 | 配置应用启动或全局插件时 |
| [api-integration.md](./api-integration.md) | API 文件组织、函数命名、请求配置、类型来源、禁止项 | 对接后端接口时 |
| [routing.md](./routing.md) | 路由模块组织、命名规则、meta 字段、路由常量 | 新增或修改路由时 |
| [authentication.md](./authentication.md) | 登录态、Token、权限展示 | 实现认证相关功能时 |
| [components.md](./components.md) | Vue SFC 结构、组件命名、组件文件夹组织、注释方法风格 | 创建 UI 组件时 |
| [hooks.md](./hooks.md) | Hooks 组织和副作用 | 抽取状态或请求逻辑时 |
| [project-capabilities.md](./project-capabilities.md) | 内置 Hooks、Utils、工具选型、缓存、路由工具、i18n 边界 | 新增页面前确认已有能力 |
| [state-management.md](./state-management.md) | Pinia Setup Store 风格、命名约定、Store 与全局 Hook | 设计复杂状态时 |
| [css-layout.md](./css-layout.md) | CSS、布局和响应式 | 实现页面布局时 |
| [styling.md](./styling.md) | 样式语言、BEM 命名、Less 变量、Prettier、ESLint 与 pre-commit | 编写样式或格式化代码时 |
| [type-safety.md](./type-safety.md) | 类型声明存放位置、命名规范、编译配置、禁止项 | 定义 API 类型时 |
| [models.md](./models.md) | Model 类型位置、命名、导入和注释要求 | 定义业务类型时 |
| [imports.md](./imports.md) | 自动导入项、强制导入来源约束、禁止手编生成文件 | 编写 import 语句时 |
| [comments.md](./comments.md) | 中文注释、JSDoc 格式、Vue 文件注释、注释密度 | 编写注释时 |
| [testing.md](./testing.md) | Jest 单元测试约定、文件位置、组件测试、jsdom | 编写测试时 |
| [quality.md](./quality.md) | 前端提交前检查 | 提交前 |

## 核心规则

| 规则 | 参考 |
| --- | --- |
| **前端类型必须来自 API 契约或生成文件，不能手写漂移类型** | [type-safety.md](./type-safety.md) |
| **统一处理后端错误码和 traceId** | [api-integration.md](./api-integration.md) |
| **认证状态由统一入口管理，禁止页面各自解析 Token** | [authentication.md](./authentication.md) |
| **组件只负责展示和交互，不直接拼接后端协议细节** | [components.md](./components.md) |
| **服务端状态优先使用请求缓存工具，不复制到全局客户端状态** | [state-management.md](./state-management.md) |

## 语言

所有文档必须使用中文编写。
