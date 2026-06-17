# Java 全栈开发指南

面向使用 Spring Boot、REST API、MyBatis-Plus 和 Maven 的生产级 Java 全栈项目开发规范。

## 结构

### [后端](./backend/index.md)

Spring Boot + REST API + MyBatis-Plus 后端开发模式：

- [目录结构](./backend/directory-structure.md)
- [Spring Boot](./backend/spring-boot.md)
- [API 模块](./backend/api-module.md)
- [API 模式](./backend/api-patterns.md)
- [数据库](./backend/database.md)
- [事务](./backend/transactions.md)
- [安全](./backend/security.md)
- [身份认证](./backend/authentication.md)
- [参数校验](./backend/validation.md)
- [错误处理](./backend/error-handling.md)
- [日志](./backend/logging.md)
- [配置](./backend/configuration.md)
- [测试](./backend/testing.md)
- [性能](./backend/performance.md)
- [依赖注入](./backend/dependency-injection.md)
- [质量检查清单](./backend/quality.md)
- [类型安全](./backend/type-safety.md)

### [前端](./frontend/index.md)

全栈项目中的前端协作规范：

- [目录结构](./frontend/directory-structure.md)
- [API 集成](./frontend/api-integration.md)
- [身份认证](./frontend/authentication.md)
- [组件](./frontend/components.md)
- [Hooks](./frontend/hooks.md)
- [状态管理](./frontend/state-management.md)
- [CSS 与布局](./frontend/css-layout.md)
- [质量检查清单](./frontend/quality.md)
- [类型安全](./frontend/type-safety.md)

### [共享](./shared/index.md)

横切关注点：

- [代码质量](./shared/code-quality.md)
- [依赖管理](./shared/dependencies.md)
- [Git 约定](./shared/git-conventions.md)
- [API 契约](./shared/api-contract.md)
- [时间规范](./shared/timestamp.md)
- [文档规范](./shared/documentation.md)

### [指南](./guides/index.md)

开发思考指南：

- [实现前检查清单](./guides/pre-implementation-checklist.md)
- [跨层思考指南](./guides/cross-layer-thinking-guide.md)
- [数据库结构变更指南](./guides/db-schema-change-guide.md)
- [事务一致性指南](./guides/transaction-consistency-guide.md)
- [生产问题排查指南](./guides/production-debugging-guide.md)
- [API 版本管理指南](./guides/api-versioning-guide.md)
- [部署检查清单](./guides/deployment-checklist.md)

### [常见问题 / 陷阱](./big-question/index.md)

Java 与 Spring Boot 常见问题和解决方案：

- [Spring 事务自调用失效](./big-question/spring-transaction-self-invocation.md)
- [JPA 懒加载边界](./big-question/jpa-lazy-loading.md)
- [N+1 查询问题](./big-question/n-plus-one-query.md)
- [时区与 Instant](./big-question/timezone-and-instant.md)
- [Jackson 序列化循环引用](./big-question/jackson-serialization-cycle.md)
- [Bean 循环依赖](./big-question/bean-circular-dependency.md)
- [连接池耗尽](./big-question/connection-pool-exhaustion.md)
- [异步上下文传递](./big-question/async-context-propagation.md)
- [CORS、CSRF 与 Cookie](./big-question/cors-csrf-cookie.md)

## 技术栈

- **后端**：Spring Boot、Spring MVC、Spring Security、MyBatis-Plus、Jakarta Bean Validation
- **数据库**：MySQL / PostgreSQL、Flyway / Liquibase、HikariCP
- **构建**：Maven、Maven Wrapper、Maven Enforcer
- **测试**：JUnit 5、Mockito、Spring Boot Test、Testcontainers
- **质量**：Alibaba Java Coding Guidelines、Checkstyle、Spotless、PMD / SonarQube
- **可观测性**：SLF4J、Logback、Actuator、Micrometer、OpenTelemetry

## 参考来源

- Alibaba Java Coding Guidelines：https://github.com/alibaba/Alibaba-Java-Coding-Guidelines
- Oracle Java Language Specification：https://docs.oracle.com/javase/specs/
- Google Java Style Guide：https://google.github.io/styleguide/javaguide.html
- Spring Boot Documentation：https://docs.spring.io/spring-boot/documentation.html
- MyBatis-Plus Documentation：https://baomidou.com/
- Apache Maven Documentation：https://maven.apache.org/guides/
- OWASP Cheat Sheet Series：https://cheatsheetseries.owasp.org/

## 用法

这些指南可用作：

1. **新项目模板** - 为新的 Java 全栈项目复制整套结构
2. **参考文档** - 实现功能时查阅具体指南
3. **代码审查清单** - 根据既定模式验证实现
4. **入职材料** - 帮助新开发者理解项目约定

## 语言

所有文档必须使用中文编写。
