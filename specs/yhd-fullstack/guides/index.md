# Java 全栈开发思考指南

> **目的**：在编码前系统性识别跨层影响、事务风险、数据库变更和生产问题。

## 可用指南

| 指南 | 目的 | 何时使用 |
| --- | --- | --- |
| [实现前检查清单](./pre-implementation-checklist.md) | 编码前确认需求、边界和复用 | 开始任何功能前 |
| [跨层思考指南](./cross-layer-thinking-guide.md) | 梳理前端、API、Service、数据库的数据流 | 功能跨越 3 层以上时 |
| [代码复用思考指南](./code-reuse-thinking-guide.md) | 新增工具、Hook、组件前判断是否已有可复用能力 | 准备新增公共能力时 |
| [缺陷根因复盘指南](./bug-root-cause-thinking-guide.md) | 修复非平凡缺陷后定位根因和补充防线 | 修复复杂 bug 后 |
| [语义变更检查清单](./semantic-change-checklist.md) | 修改字段含义、枚举、状态码时确保所有依赖点同步 | 修改数据语义时 |
| [数据库结构变更指南](./db-schema-change-guide.md) | 确保表结构、代码、数据和部署一致 | 修改数据库结构时 |
| [事务一致性指南](./transaction-consistency-guide.md) | 设计多表写入和失败补偿 | 实现复杂写操作时 |
| [生产问题排查指南](./production-debugging-guide.md) | 有序定位线上故障 | 处理生产异常时 |
| [API 版本管理指南](./api-versioning-guide.md) | 管理兼容和破坏性变更 | 修改公开 API 时 |
| [部署检查清单](./deployment-checklist.md) | 发布前确认配置、迁移和监控 | 上线前 |

## 快速选择

- 新功能动手前：先读 [实现前检查清单](./pre-implementation-checklist.md)。
- 需求跨页面、Store、接口或路由：读 [跨层思考指南](./cross-layer-thinking-guide.md)。
- 准备新增公共能力：先读 [代码复用思考指南](./code-reuse-thinking-guide.md)，并检索现有代码。
- 修复复杂 bug 后：读 [缺陷根因复盘指南](./bug-root-cause-thinking-guide.md)，确认是否需要补测试或规则。
- 字段或状态语义变化：读 [语义变更检查清单](./semantic-change-checklist.md)，避免只改一处。

## Java 全栈典型层次

```text
前端页面 / 组件
        |
        v
REST API / Controller
        |
        v
Service / 事务边界
        |
        v
Mapper / MyBatis-Plus
        |
        v
数据库 / 缓存 / 外部系统
```

## 核心原则

1. **先搜索再新增**：新增常量、枚举、工具、接口前先搜索已有模式。
2. **先契约再实现**：跨前后端功能先确定 API 契约。
3. **先边界再事务**：写操作先明确一致性边界。
4. **先数据再代码**：字段语义和迁移策略决定代码实现。
5. **先观测再上线**：没有日志、指标和追踪的功能不能放心上线。

## 语言

所有文档必须使用中文编写。
