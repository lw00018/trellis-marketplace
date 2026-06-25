# API 模块规范

## 分层职责

| 层 | 职责 | 禁止事项 |
| --- | --- | --- |
| Controller | HTTP 协议适配、参数校验、权限入口、响应转换 | 写业务规则、直接访问数据库 |
| Service | 业务规则、事务边界、跨资源编排 | 依赖 Web 请求对象、拼接 SQL |
| Mapper | 数据库访问、MyBatis-Plus 查询 | 写业务判断、调用远程接口 |
| Entity | 数据库表映射 | 暴露给前端、承载复杂业务逻辑 |
| RequestVO / ResponseVO | Controller 入参和响应契约 | 复用 DTO、Entity 或混用请求响应对象 |
| DTO | 业务层内部数据传输 | 直接作为 Controller 入参或响应对象 |
| Convert Mapper | VO、DTO、Entity 之间的对象转换 | 写业务规则、访问数据库 |

## Controller 规则

- 每个 Controller 面向一个资源或聚合根。
- 路径使用名词复数，例如 `/api/v1/users`。
- Controller 方法入参必须使用 `{业务名}RequestVO`。
- Controller 方法响应必须使用 `{业务名}ResponseVO` 或统一分页响应中的 `{业务名}ResponseVO`。
- 不得返回 MyBatis-Plus `Page`、Entity、DTO 或内部异常对象。

### Swagger 3 注解

- Controller 类必须加 `@Tag(name, description)`，按资源或聚合根分组。
- Controller 方法必须加 `@Operation(summary, description)`，描述接口语义。
- Controller 方法参数必须加 `@Parameter(description, required)`，`@PathVariable`、`@RequestParam`、`@RequestBody` 均覆盖。
- VO、DTO、Entity 字段必须加 `@Schema(description, example)`；枚举字段加 `allowableValues`，必填字段加 `requiredMode = RequiredMode.REQUIRED`。

```java
@Tag(name = "用户管理", description = "用户创建、查询、状态变更接口")
@RestController
@RequestMapping("/api/v1/users")
public class UserController {

    @Operation(summary = "创建用户", description = "根据请求参数创建用户并返回详情")
    @PostMapping
    public ResponseResult<UserDetailResponseVO> createUser(
            @Parameter(description = "创建用户请求体", required = true)
            @Valid @RequestBody CreateUserRequestVO requestVO) {
    }
}
```

```java
@Schema(description = "创建用户请求")
public class CreateUserRequestVO extends BaseVO {

    @Schema(description = "用户名", example = "zhangsan", requiredMode = RequiredMode.REQUIRED)
    private String name;

    @Schema(description = "用户类型", example = "ADMIN", allowableValues = {"ADMIN", "USER"}, requiredMode = RequiredMode.REQUIRED)
    private String userType;
}
```

## Service 规则

- 事务边界放在 Service 公共方法上。
- Service 方法名称表达业务动作，例如 `createUser`、`disableAccount`。
- 复杂流程拆分为私有方法或领域服务，避免一个方法过长。
- 跨模块调用优先依赖接口，不直接依赖对方内部 Mapper。

## Mapper 规则

- 数据访问 Mapper 只负责数据库访问，禁止包含业务流程判断。
- 数据访问 Mapper 简单 CRUD 优先使用 MyBatis-Plus `BaseMapper`。
- 数据访问 Mapper 复杂 SQL 使用 XML，并配套单元或集成测试。
- XML 路径统一为 `classpath*:/mapper/**/*.xml`。
- 对象转换 Mapper 只负责 RequestVO、ResponseVO、DTO、Entity 之间的字段映射，不得访问数据库或编排业务流程。
- 使用 MapStruct 的对象转换 Mapper 必须使用 `@Mapper(componentModel = "spring")` 注册为 Spring Bean。
- 对象转换 Mapper 命名为 `{业务名}ConvertMapper`，例如 `UserConvertMapper`。

```java
@Mapper(componentModel = "spring")
public interface UserConvertMapper {

    UserDTO toDTO(CreateUserRequestVO requestVO);

    UserResponseVO toResponseVO(UserDTO userDTO);

    User toEntity(UserDTO userDTO);
}
```

## VO / DTO 转换

- RequestVO 到 DTO、DTO 到 Entity、Entity 到 DTO、DTO 到 ResponseVO 的转换必须集中处理。
- 简单转换可以手写，复杂转换必须使用 MapStruct 或统一转换器，禁止在 Controller 中散落字段复制逻辑。
- Controller 只负责接收 RequestVO、调用 Service、返回 ResponseVO。
- Service 只接收和返回 DTO，不依赖 RequestVO、ResponseVO 或 Web 请求对象。
- Mapper 转换字段存在名称差异、枚举转换、时间格式转换或金额单位转换时，必须显式声明映射规则并覆盖测试。

## 转换测试

- 每个对象转换 Mapper 必须建立单元测试，覆盖 RequestVO 到 DTO、DTO 到 ResponseVO、DTO 到 Entity 的核心路径。
- 测试必须验证字段完整性、空值策略、枚举映射、金额单位、时间格式和集合字段。
- 新增或重命名 VO / DTO 字段时必须同步更新转换测试。
- 转换测试不得依赖数据库、网络或 Spring 全量上下文。

## 模块提交检查

- 新 API 是否有 RequestVO、ResponseVO、DTO、参数校验和错误码。
- Controller、Service、数据访问 Mapper、对象转换 Mapper 的职责是否分离。
- 对象转换 Mapper 是否使用 `@Mapper(componentModel = "spring")` 正确注册。
- VO 与 DTO 之间的转换测试是否覆盖核心字段和边界值。
- 重命名数据对象后，控制器、服务层、数据访问层和测试引用是否同步调整。
- API 是否有测试覆盖正常、异常和权限路径。
