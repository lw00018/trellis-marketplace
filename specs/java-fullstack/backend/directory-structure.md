# 后端目录结构

## 目标

目录结构必须让业务边界、依赖方向和代码职责一眼可见。Spring Boot 项目采用按类型分层、类型目录内按业务模块分包的结构，不采用一个业务模块一个顶级文件夹的存储方式。

## 推荐结构

```text
src/main/java/com/example/app/
├── Application.java
├── common/
│   ├── exception/
│   ├── response/
│   ├── security/
│   └── util/
├── config/
├── controller/
│   ├── user/
│   │   └── UserController.java
│   └── order/
├── service/
│   ├── user/
│   │   ├── UserService.java
│   │   └── impl/UserServiceImpl.java
│   └── order/
├── mapper/
│   ├── user/
│   └── order/
├── entity/
│   ├── user/
│   └── order/
├── dto/
│   ├── user/
│   └── order/
└── vo/
    ├── user/
    └── order/
```

## 强制规则

- 主启动类必须位于根包，保证组件扫描覆盖项目代码且不越界。
- 顶级业务代码目录必须按类型划分为 `controller`、`service`、`mapper`、`entity`、`dto`、`vo`，不得按业务模块创建顶级目录。
- 各类型目录内必须按领域模块继续分包，例如 `user`、`order`、`payment`，不得使用含糊的 `business`、`module`、`manager`。
- `controller` 只放接口适配代码，`service` 放业务编排，`mapper` 放持久化访问，`entity` 只映射数据库表。
- `vo` 用于接口入参和响应契约，`dto` 用于业务层内部传输，不得把 Entity 直接暴露给前端。
- 请求入参对象统一命名为 `{业务名}RequestVO`，例如 `CreateUserRequestVO`、`UpdateUserRequestVO`。
- 响应对象统一命名为 `{业务名}ResponseVO`，例如 `UserDetailResponseVO`、`UserListItemResponseVO`。
- 业务层数据传输对象统一命名为 `{业务名}DTO`，例如 `UserDTO`、`OrderSettlementDTO`。
- Entity（PO）必须继承 `com.yhd.common.pojo.po.BaseEntity`。`BaseEntity` 继承自 `BaseObject`（实现 `Serializable`），提供 `id`（UUID 自动赋值）、`createdBy`、`createdDate`（INSERT 自动填充）、`updatedBy`、`updatedDate`（INSERT_UPDATE 自动填充）公共字段，基于 MyBatis-Plus。
- DTO 必须继承 `com.yhd.common.pojo.dto.BaseDTO`。`BaseDTO` 继承自 `BaseObject`（实现 `Serializable`）。
- VO 必须继承 `com.yhd.common.pojo.vo.BaseVO`。`BaseVO` 继承自 `BaseObject`（实现 `Serializable`），提供 `isBlank(value, key)` 空值校验日志方法。
- DO、DTO、VO、BO、AO 等数据对象必须使用 Swagger 3 注解（`@Schema`）标注类和字段描述。
- 公共能力放入 `common` 或独立 starter，不得把业务逻辑塞进 util。
- 配置类放入 `config`，只承载基础设施配置，不承载业务规则。

## 命名约定

| 类型 | 命名 |
| --- | --- |
| Controller | `UserController` |
| Service 接口 | `UserService` |
| Service 实现 | `UserServiceImpl` |
| 数据访问 Mapper | `UserMapper` |
| 对象转换 Mapper | `UserConvertMapper` |
| Entity（PO） | `User` |
| 请求 VO | `CreateUserRequestVO`、`UpdateUserRequestVO` |
| 响应 VO | `UserDetailResponseVO`、`UserListItemResponseVO` |
| 业务 DTO | `UserDTO`、`OrderSettlementDTO` |

## 依赖方向

```text
controller -> RequestVO / ResponseVO
controller -> service
service -> DTO / domain object
service -> mapper -> database
mapper -> entity
convert mapper -> RequestVO / ResponseVO / DTO / Entity
```

禁止反向依赖：Mapper 不得调用 Service，Entity 不得依赖 Controller、Service 或 Web 框架。

## 多模块 Maven 项目

```text
app-parent/
├── pom.xml
├── app-common/
├── app-api/
├── app-service/
├── app-infrastructure/
└── app-web/
```

- 父 POM 只管理版本、插件和公共配置。
- `app-common` 不得依赖业务模块。
- `app-api` 放对外契约、DTO、枚举和客户端接口。
- `app-service` 放业务规则。
- `app-infrastructure` 放数据库、消息、外部系统适配。
- `app-web` 放启动类、Controller 和 Web 配置。
