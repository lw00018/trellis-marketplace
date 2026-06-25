# 类型安全规范

## 泛型

- 禁止使用裸泛型，例如 `List`、`Map`。
- 公共方法必须声明明确泛型边界。
- 未检查强转必须局部化，并解释来源可信性。
- 不要把 `Object` 作为业务数据通用容器。

## Optional

- Optional 可用于返回值表达可能不存在。
- 禁止把 Optional 作为字段、DTO 属性或方法参数。
- 禁止直接调用 `get()`，必须使用 `orElse`、`orElseThrow` 或分支处理。

## 枚举

- 有固定取值的业务字段必须使用枚举。
- 枚举必须有稳定 code，不依赖 name 作为持久化契约。
- 删除或修改枚举语义属于破坏性变更。
- 前后端共享枚举必须通过 API 契约同步。

## 金额和时间

- 金额使用 `BigDecimal`，禁止使用 `float` 或 `double`。
- 时间使用 `java.time`，禁止新业务使用 `Date` 和 `Calendar`。
- 跨系统传输时间使用 ISO-8601 或 Unix 毫秒，并明确时区。

## VO / DTO 边界

- Entity、RequestVO、ResponseVO、DTO 不得混用。
- RequestVO 只用于 Controller 入参，ResponseVO 只用于 Controller 响应，DTO 只用于业务层内部传输。
- 对外 API 不暴露 Entity、DTO、MyBatis-Plus `Page` 或内部异常对象。
- 类型转换必须集中处理，减少散落转换造成字段遗漏。

## 空值

- 明确区分不存在、空集合、空字符串和默认值。
- 集合返回空集合，不返回 null。
- 公共方法入参必须通过校验或断言明确空值处理策略。
