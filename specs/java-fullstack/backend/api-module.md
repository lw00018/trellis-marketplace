# API 模块规范

## 分层职责

| 层 | 职责 | 禁止事项 |
| --- | --- | --- |
| Controller | HTTP 协议适配、参数校验、权限入口、响应转换 | 写业务规则、直接访问数据库 |
| Service | 业务规则、事务边界、跨资源编排 | 依赖 Web 请求对象、拼接 SQL |
| Mapper | 数据库访问、MyBatis-Plus 查询 | 写业务判断、调用远程接口 |
| Entity | 数据库表映射 | 暴露给前端、承载复杂业务逻辑 |
| DTO / VO | 请求和响应契约 | 混用请求响应对象 |

## Controller 规则

- 每个 Controller 面向一个资源或聚合根。
- 路径使用名词复数，例如 `/api/v1/users`。
- Controller 方法必须声明清晰的请求 DTO 和响应 VO。
- 不得返回 MyBatis-Plus `Page`、Entity 或内部异常对象。

## Service 规则

- 事务边界放在 Service 公共方法上。
- Service 方法名称表达业务动作，例如 `createUser`、`disableAccount`。
- 复杂流程拆分为私有方法或领域服务，避免一个方法过长。
- 跨模块调用优先依赖接口，不直接依赖对方内部 Mapper。

## Mapper 规则

- Mapper 只负责数据访问，禁止包含业务流程判断。
- 简单 CRUD 优先使用 MyBatis-Plus BaseMapper。
- 复杂 SQL 使用 XML，并配套单元或集成测试。
- XML 路径统一为 `classpath*:/mapper/**/*.xml`。

## DTO 转换

- 请求 DTO 到业务命令对象、Entity 到 VO 的转换必须集中处理。
- 简单转换可以手写，复杂转换可以使用 MapStruct。
- 禁止在 Controller 中散落大量字段复制逻辑。

## 模块提交检查

- 新 API 是否有请求 DTO、响应 VO、参数校验和错误码。
- Service 是否承载业务规则和事务边界。
- Mapper 是否只处理数据访问。
- API 是否有测试覆盖正常、异常和权限路径。
