# Java / Spring Boot 常见问题与陷阱

## 问题列表

| 问题 | 说明 | 何时阅读 |
| --- | --- | --- |
| [Spring 事务自调用失效](./spring-transaction-self-invocation.md) | `@Transactional` 不生效的常见原因 | 事务异常或回滚失败时 |
| [JPA 懒加载边界](./jpa-lazy-loading.md) | Session 关闭后访问懒加载属性失败 | 使用 JPA / Hibernate 时 |
| [N+1 查询问题](./n-plus-one-query.md) | 循环查询导致性能雪崩 | 列表详情聚合查询时 |
| [时区与 Instant](./timezone-and-instant.md) | 时间偏移、秒毫秒混用、夏令时 | 处理时间字段时 |
| [Jackson 序列化循环引用](./jackson-serialization-cycle.md) | 双向关联对象序列化栈溢出 | 返回复杂对象时 |
| [Bean 循环依赖](./bean-circular-dependency.md) | Spring Bean 互相依赖启动失败 | 设计服务依赖时 |
| [连接池耗尽](./connection-pool-exhaustion.md) | 数据库连接等待、接口超时 | 高并发或慢 SQL 时 |
| [异步上下文传递](./async-context-propagation.md) | traceId、用户上下文在线程切换后丢失 | 使用异步任务时 |
| [CORS、CSRF 与 Cookie](./cors-csrf-cookie.md) | 跨域、Cookie、CSRF 策略混乱 | 前后端分离认证时 |

## 使用方式

- 遇到问题先定位是否属于这些经典陷阱。
- 修复后把项目特有根因补充到对应文档。
- 不要只修症状，必须补充测试、监控或规范防止复发。
