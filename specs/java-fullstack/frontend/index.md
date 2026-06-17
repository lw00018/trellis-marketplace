# 前端开发指南索引

> **定位**：服务于 Java 全栈项目的前端协作规范，重点保证 API 契约、认证状态、错误处理和类型边界一致。

## 文档文件

| 文件 | 说明 | 何时阅读 |
| --- | --- | --- |
| [directory-structure.md](./directory-structure.md) | 前端目录组织 | 新建页面或模块时 |
| [api-integration.md](./api-integration.md) | REST API 调用、错误处理、分页 | 对接后端接口时 |
| [authentication.md](./authentication.md) | 登录态、Token、权限展示 | 实现认证相关功能时 |
| [components.md](./components.md) | 组件职责和复用 | 创建 UI 组件时 |
| [hooks.md](./hooks.md) | Hooks 组织和副作用 | 抽取状态或请求逻辑时 |
| [state-management.md](./state-management.md) | 服务端状态和客户端状态 | 设计复杂状态时 |
| [css-layout.md](./css-layout.md) | CSS、布局和响应式 | 实现页面布局时 |
| [type-safety.md](./type-safety.md) | 前端类型边界 | 定义 API 类型时 |
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
