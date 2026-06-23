# 后端开发指南索引

> **技术栈**：Spring Boot + Spring MVC + REST API + MyBatis-Plus + Maven

## 相关指南

| 指南 | 位置 | 何时阅读 |
| --- | --- | --- |
| **共享代码规范** | `../shared/` | 始终适用，覆盖所有代码 |
| **开发思考指南** | `../guides/` | 实现跨层功能、数据库变更、生产排查前 |

---

## 文档文件

| 文件 | 说明 | 何时阅读 |
| --- | --- | --- |
| [directory-structure.md](./directory-structure.md) | 包结构、分层和模块布局 | 新建功能或模块时 |
| [spring-boot.md](./spring-boot.md) | Spring Boot 启动、自动配置、Profile、Actuator | 配置应用基础设施时 |
| [api-module.md](./api-module.md) | Controller、Service、Mapper、DTO 的模块组织 | 创建业务 API 时 |
| [api-patterns.md](./api-patterns.md) | REST 设计、分页、状态码、错误响应 | 设计或修改接口时 |
| [database.md](./database.md) | MyBatis-Plus、SQL、索引、迁移、批处理 | 处理数据库读写时 |
| [transactions.md](./transactions.md) | 事务边界、传播行为、自调用陷阱 | 实现写操作和一致性逻辑时 |
| [security.md](./security.md) | 输入安全、敏感数据、OWASP 风险 | 处理外部输入和权限边界时 |
| [authentication.md](./authentication.md) | 登录、Token、Session、权限模型 | 实现认证授权时 |
| [validation.md](./validation.md) | Bean Validation、业务校验、错误提示 | 处理请求参数和领域规则时 |
| [error-handling.md](./error-handling.md) | 异常分层、错误码、全局异常处理 | 处理失败路径时 |
| [logging.md](./logging.md) | 结构化日志、traceId、异常日志 | 排查问题和接入可观测性时 |
| [configuration.md](./configuration.md) | 配置项、环境变量、密钥、Profile | 新增或调整配置时 |
| [testing.md](./testing.md) | 单元测试、集成测试、Testcontainers | 编写或修改测试时 |
| [performance.md](./performance.md) | 连接池、缓存、批处理、异步、慢查询 | 性能优化时 |
| [dependency-injection.md](./dependency-injection.md) | 构造器注入、Bean 命名、多实现策略 | 设计组件依赖时 |
| [type-safety.md](./type-safety.md) | 泛型、枚举、Optional、DTO 类型边界 | 处理类型设计时 |
| [quality.md](./quality.md) | 后端提交前检查清单 | 提交代码前 |

---

## 快速导航

### 模块结构

| 任务 | 文件 |
| --- | --- |
| 设计包结构 | [directory-structure.md](./directory-structure.md) |
| 新建业务模块 | [api-module.md](./api-module.md) |
| 划分 Controller / Service / Mapper | [api-module.md](./api-module.md) |
| 选择领域分包或技术分包 | [directory-structure.md](./directory-structure.md) |

### API 设计

| 任务 | 文件 |
| --- | --- |
| REST 资源命名 | [api-patterns.md](./api-patterns.md) |
| 分页和排序 | [api-patterns.md](./api-patterns.md) |
| 请求参数校验 | [validation.md](./validation.md) |
| 统一响应和错误码 | [api-patterns.md](./api-patterns.md) |
| 版本管理 | [../guides/api-versioning-guide.md](../guides/api-versioning-guide.md) |

### 数据库与事务

| 任务 | 文件 |
| --- | --- |
| MyBatis-Plus 实体和 Mapper | [database.md](./database.md) |
| Wrapper 安全使用 | [database.md](./database.md) |
| 数据库迁移 | [database.md](./database.md) |
| 事务边界设计 | [transactions.md](./transactions.md) |
| 多表写入一致性 | [../guides/transaction-consistency-guide.md](../guides/transaction-consistency-guide.md) |

### 安全与认证

| 任务 | 文件 |
| --- | --- |
| 登录和 Token | [authentication.md](./authentication.md) |
| RBAC 权限 | [authentication.md](./authentication.md) |
| 输入安全 | [security.md](./security.md) |
| CORS / CSRF / Cookie | [../big-question/cors-csrf-cookie.md](../big-question/cors-csrf-cookie.md) |

### 可观测性与质量

| 任务 | 文件 |
| --- | --- |
| 结构化日志 | [logging.md](./logging.md) |
| 异常分层 | [error-handling.md](./error-handling.md) |
| 性能优化 | [performance.md](./performance.md) |
| 测试策略 | [testing.md](./testing.md) |
| 提交前检查 | [quality.md](./quality.md) |

---

## 核心规则摘要

| 规则 | 参考 |
| --- | --- |
| **Controller 只处理协议转换、参数校验和权限入口** | [api-module.md](./api-module.md) |
| **业务规则必须放在 Service 层，不写在 Controller 或 Mapper 中** | [api-module.md](./api-module.md) |
| **Controller 入参使用 RequestVO，响应使用 ResponseVO，Service 使用 DTO** | [api-module.md](./api-module.md) |
| **MyBatis-Plus 优先使用 LambdaQueryWrapper / LambdaUpdateWrapper** | [database.md](./database.md) |
| **更新和删除必须带明确条件，禁止无条件全表操作** | [database.md](./database.md) |
| **事务边界放在 Service 公共方法上，避免自调用事务失效** | [transactions.md](./transactions.md) |
| **所有外部输入必须校验，不能信任前端传入的字段名、排序字段或 SQL 片段** | [security.md](./security.md) |
| **异常必须统一转换为错误码和可理解消息** | [error-handling.md](./error-handling.md) |
| **日志必须参数化输出，禁止记录密码、Token、身份证、银行卡等敏感信息** | [logging.md](./logging.md) |
| **配置项使用 @ConfigurationProperties，密钥不得提交到仓库** | [configuration.md](./configuration.md) |
| **生产环境 Actuator 只暴露必要端点** | [spring-boot.md](./spring-boot.md) |
| **提交前必须运行 Maven 校验和测试** | [quality.md](./quality.md) |

---

## 典型代码位置

| 功能 | 典型位置 |
| --- | --- |
| 启动类 | `src/main/java/{basePackage}/Application.java` |
| REST Controller | `src/main/java/{basePackage}/{domain}/controller/` |
| Service 接口和实现 | `src/main/java/{basePackage}/{domain}/service/` |
| MyBatis-Plus Mapper | `src/main/java/{basePackage}/{domain}/mapper/` |
| Entity | `src/main/java/{basePackage}/{domain}/entity/` |
| RequestVO / ResponseVO | `src/main/java/{basePackage}/{domain}/vo/` |
| DTO | `src/main/java/{basePackage}/{domain}/dto/` |
| 对象转换 Mapper | `src/main/java/{basePackage}/{domain}/mapper/` 或 `convert/` |
| 配置类 | `src/main/java/{basePackage}/config/` |
| 全局异常处理 | `src/main/java/{basePackage}/common/exception/` |
| XML Mapper | `src/main/resources/mapper/**/*.xml` |
| 数据库迁移 | `src/main/resources/db/migration/` |
| 测试代码 | `src/test/java/` |

---

## 语言

所有文档必须使用中文编写。
