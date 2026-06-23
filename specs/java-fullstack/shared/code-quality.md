# 代码质量规范

## 规范来源

- 阿里巴巴 Java 开发手册中的强制规则作为最低基线。
- Oracle Java 规范和 Google Java Style Guide 作为命名、格式和可读性参考。
- 团队规则可以更严格，但不得低于安全和质量底线。

## 命名

- 类名使用 UpperCamelCase。
- 方法名、参数名、局部变量名使用 lowerCamelCase。
- 常量使用 UPPER_SNAKE_CASE。
- 禁止中文命名、拼音和英文混用、无意义缩写。
- RequestVO、ResponseVO、DTO、DO、BO、AO 等后缀必须语义一致。

## 可读性

- 方法只做一层抽象，不混合协议、业务、持久化细节。
- 条件复杂时提取有语义的方法。
- 注释解释原因和约束，不重复代码。
- TODO 必须带负责人或问题编号。

## 注释

### 总体原则

- 注释必须解释业务意图、设计原因、边界条件、兼容约束或外部依赖，不复述代码表面行为。
- 优先通过清晰命名、拆分方法和类型建模表达含义；只有代码本身无法完整表达上下文时才添加注释。
- 公共 API、复杂业务规则、非显而易见的算法、外部系统对接和临时兼容逻辑必须有注释或文档。
- 注释必须随代码同步更新；过期注释等同于错误代码。
- 禁止提交无意义注释、注释掉的废弃代码和只翻译代码的注释。

### 类注释

- 对外暴露的 Controller、Service、组件、配置类、DTO、VO、事件和枚举必须说明职责、适用范围和关键约束。
- 内部简单实现类可以不写类注释，但类名必须准确表达职责。
- 类注释使用 Javadoc，第一句话概括职责，后续说明业务边界或使用限制。

```java
/**
 * 订单结算服务。
 *
 * <p>负责计算订单应付金额、优惠抵扣和运费，不负责创建支付单。</p>
 */
@Service
public class OrderSettlementService {
}
```

### 字段注释

- 公共契约字段、数据库映射字段、配置项、枚举值和含单位、格式、取值范围的字段必须注释。
- 字段注释必须说明业务含义、单位、格式、默认值或取值限制。
- 禁止写“用户 ID”“订单状态”这类与字段名完全重复的注释，除非需要补充来源、范围或兼容含义。

```java
/**
 * 优惠券抵扣金额，单位：分，不能为负数。
 */
private Long couponDiscountAmount;

/**
 * 外部支付渠道交易号，由支付网关生成，可用于对账。
 */
private String channelTradeNo;
```

### 方法注释

- 公共方法、跨模块调用方法、复杂业务方法和有副作用的方法必须使用 Javadoc 说明行为、参数约束、返回值语义和异常条件。
- 私有方法只有在业务规则复杂、算法不直观或存在特殊边界时才需要注释。
- 方法注释必须说明“做什么”和“为什么这样做”，不逐行描述实现过程。

```java
/**
 * 锁定订单库存。
 *
 * <p>库存锁定成功后必须在支付超时或订单取消时释放，避免长期占用可售库存。</p>
 *
 * @param orderId 订单 ID，必须是待支付订单
 * @throws BusinessException 库存不足或订单状态不允许锁定时抛出
 */
public void lockInventory(Long orderId) {
}
```

### 方法内部注释

- 方法内部只在关键分支、复杂计算、性能权衡、并发控制、事务边界、兼容逻辑和临时方案处添加注释。
- 注释放在被解释代码的上一行，说明原因或约束。
- 禁止为简单赋值、普通循环、显而易见的条件判断添加注释。
- TODO、FIXME 必须带负责人或问题编号，并说明移除条件。

```java
public BigDecimal calculateRefundAmount(Order order) {
    // 按支付渠道要求，退款金额必须按订单维度四舍五入，不能按商品行分别取整。
    BigDecimal refundAmount = order.getItems().stream()
            .map(this::calculateItemRefundAmount)
            .reduce(BigDecimal.ZERO, BigDecimal::add)
            .setScale(2, RoundingMode.HALF_UP);

    // TODO(order-1421): 支付网关完成幂等升级后移除本地防重校验。
    checkDuplicateRefund(order.getId());

    return refundAmount;
}
```

### 不推荐示例

```java
// 设置用户名
user.setName(name);

// 如果订单状态是已取消，则返回 false
if (order.isCancelled()) {
    return false;
}
```

## Lombok

- 项目使用 Lombok，常用注解：`@Data`、`@Getter`/`@Setter`、`@Builder`、`@NoArgsConstructor`、`@AllArgsConstructor`、`@Slf4j`。
- 继承 `BaseEntity` / `BaseVO` / `BaseDTO` 的类使用 `@EqualsAndHashCode(callSuper = true)`。
- 禁止使用 `@Value`（不可变类用 `record` 替代）和 `@SneakyThrows`。

## 质量工具

- IDE 安装 Alibaba Java Coding Guidelines 插件。
- CI 接入 Checkstyle、Spotless、PMD 或 SonarQube。
- 强制规则失败必须阻断合并。
