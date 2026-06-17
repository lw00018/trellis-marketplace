# 共享开发指南

> 这些规范适用于 Java 全栈项目中的所有代码、文档、依赖和接口契约。

## 文档文件

| 文件 | 说明 | 何时阅读 |
| --- | --- | --- |
| [code-quality.md](./code-quality.md) | 代码质量强制规则 | 始终 |
| [dependencies.md](./dependencies.md) | Maven 依赖和版本管理 | 新增或升级依赖时 |
| [git-conventions.md](./git-conventions.md) | Git 分支和提交约定 | 提交前 |
| [api-contract.md](./api-contract.md) | 前后端 API 契约 | 修改接口时 |
| [timestamp.md](./timestamp.md) | 时间和时区规范 | 处理时间字段时 |
| [documentation.md](./documentation.md) | 文档维护规则 | 新增规范或功能说明时 |

## 核心规则

| 规则 | 文件 |
| --- | --- |
| **阿里 Java 强制规则作为 CI 阻断项** | [code-quality.md](./code-quality.md) |
| **Maven 版本由 dependencyManagement 统一管理** | [dependencies.md](./dependencies.md) |
| **API 变更必须同步契约、前端、后端和测试** | [api-contract.md](./api-contract.md) |
| **跨系统时间使用 ISO-8601 或 Unix 毫秒并明确时区** | [timestamp.md](./timestamp.md) |
| **提交信息必须表达变更意图和影响范围** | [git-conventions.md](./git-conventions.md) |

## 每次提交前

- [ ] `mvn -B clean verify` 通过。
- [ ] 没有提交密钥、Token、证书和生产配置。
- [ ] API 契约变更已同步前端和文档。
- [ ] 数据库变更有迁移脚本。
- [ ] 日志不包含敏感信息。

## 语言

所有文档必须使用中文编写。
