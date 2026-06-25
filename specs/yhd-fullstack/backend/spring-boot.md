# Spring Boot 规范

## 技术栈

所有新建模块必须遵守以下规则：

| 项目       | 值                                             |
| -------- | --------------------------------------------- |
| 继承父 POM  | `com.yhd.springcloud:yhd-spring-cloud-parent` |
| 父 POM 版本 | `3.0.6`（所有子模块不需要指定依赖版本）                       |
| Java 版本  | 17                                            |
| 组织 GID   | `com.yhd`                                     |
| 初始版本     | `1.0.0-SNAPSHOT`                              |

## 启动类

- 启动类必须放在根包下，名称使用 `Application` 或 `{ProjectName}Application`。
- 禁止在启动类中编写业务初始化逻辑。
- 需要启动后执行的任务使用 `ApplicationRunner` 或 `CommandLineRunner`，并明确环境限制和幂等性。

## Profile

- 必须至少区分 `local`、`test`、`prod`。
- 本地配置可以有默认值，生产配置必须通过环境变量、配置中心或密钥系统注入。
- 禁止在代码中通过 `if (profile.equals("prod"))` 分散环境判断，优先使用配置和 Bean 条件装配。

## 自动配置

- 优先使用 Spring Boot Starter 管理基础设施。
- 自定义 starter 必须提供清晰的 auto-configuration、properties 和条件装配。
- 禁止依赖隐式 Bean 覆盖解决冲突；存在多个候选 Bean 时必须显式命名或使用策略模式。

## Actuator

生产环境只暴露必要端点：

```yaml
management:
  endpoints:
    web:
      exposure:
        include: health,info,prometheus
  endpoint:
    health:
      show-details: when_authorized
```

- `env`、`heapdump`、`beans`、`loggers` 不得暴露到公网。
- 健康检查必须覆盖数据库、缓存、消息队列等关键依赖。
- 指标命名必须稳定，避免在标签中放高基数字段。

## 配置绑定

- 配置项优先使用 `@ConfigurationProperties`，不得到处散落 `@Value`。
- 配置类必须启用校验，关键配置缺失时启动失败。
- 配置命名使用小写 kebab-case，例如 `app.security.jwt-secret`。

## 项目依赖

使用 Spring Cloud Alibaba 体系，基础设施能力通过公司 Starter 引入，禁止自行引入替代实现。

### 基础依赖（所有服务必须引入）

| 用途          | groupId                    | artifactId                                     | 说明                          |
| ----------- | -------------------------- | ---------------------------------------------- | --------------------------- |
| Nacos 配置中心  | `com.alibaba.cloud`        | `spring-cloud-starter-alibaba-nacos-config`    | 配置管理                        |
| Nacos 服务发现  | `com.alibaba.cloud`        | `spring-cloud-starter-alibaba-nacos-discovery` | 服务注册                        |
| Web 模块      | `org.springframework.boot` | `spring-boot-starter-web`                      | 排除 Tomcat                   |
| Undertow 容器 | `org.springframework.boot` | `spring-boot-starter-undertow`                 | 替代 Tomcat，性能更好              |
| API 文档      | `com.yhd`                  | `yhd-springdoc-starter`                        | Swagger 3 / Springdoc       |
| 数据校验        | `com.yhd`                  | `yhd-validator-starter`                        | Bean Validation             |
| XSS 防护      | `com.yhd`                  | `yhd-xss-starter`                              | 输入过滤                        |
| Lombok      | `org.projectlombok`        | `lombok`                                       | `<optional>true</optional>` |
| Hutool 工具库  | `cn.hutool`                | `hutool-all`                                   | 通用工具                        |
| 链路追踪       | `com.yhd`                  | `yhd-skywalking-starter`                       | 日志记录及 trace_id             |
| 数据库连接       | `com.yhd`                  | `yhd-database-starter`                         | 已内置 MyBatis-Plus            |
| MySQL 驱动    | `mysql`                    | `mysql-connector-java`                         | 数据库驱动                       |

### 业务依赖（按需引入）

| 用途       | artifactId                  | 说明             |
| -------- | --------------------------- | -------------- |
| Redis 缓存 | `yhd-redis-starter`         | 缓存和分布式会话       |
| 动态数据源    | `yhd-dynamic-database`      | 多数据源切换         |
| Excel 操作 | `yhd-easyexcel-starter`     | 表格导入导出         |
| 定时任务     | `yhd-job-starter`           | 分布式定时调度        |
| 分布式锁和幂等性 | `yhd-lock-starter`          | 幂等和防重          |
| 文件上传     | `yhd-oss-starter`           | 对象存储           |
| 消息队列     | `yhd-rocketmq-starter`      | RocketMQ       |
| 分布式事务    | `yhd-seata-starter`         | Seata AT 模式    |
| 序号生成     | `yhd-sequence-starter`      | 业务编码生成         |
| 分库分表     | `yhd-sharding-jdbc-starter` | ShardingSphere |

### 依赖规则

- Web 容器使用 Undertow，必须排除 `spring-boot-starter-tomcat`。
- 基础设施能力统一使用 `yhd-*-starter`，禁止自行引入原生依赖（如直接引入 `mybatis-plus`、`springdoc-openapi` 等）。
- 业务依赖按需引入，不使用的 Starter 不要加。

## 生产默认值

- 线程池、连接池、超时时间、重试次数必须显式配置。
- 外部调用必须设置连接超时、读取超时和总超时。
- 默认禁止无限重试、无限队列和无限等待。

