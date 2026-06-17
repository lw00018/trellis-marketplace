# 日志规范

## 日志框架

- 使用 Lombok `@Slf4j` 注解，禁止手动声明 logger。
- 底层实现使用 Logback 或项目统一实现。
- 禁止直接使用 `System.out.println`。
- 禁止在业务代码中依赖具体日志实现类。

## 日志级别

| 级别 | 用途 |
| --- | --- |
| ERROR | 系统错误，必须包含异常堆栈 |
| WARN | 潜在问题（降级、重试） |
| INFO | 关键业务流程、外部调用入参/出参 |
| DEBUG | 调试细节，生产默认关闭 |
| TRACE | 极细粒度诊断信息，谨慎使用 |

## 参数化输出

必须使用参数化日志，禁止字符串拼接：

```java
@Slf4j
public class OrderServiceImpl {

    public Order createOrder(OrderDTO dto) {
        log.info("开始创建订单，userId={}, skuId={}, quantity={}",
                 dto.getUserId(), dto.getSkuId(), dto.getQuantity());

        try {
            Order order = doCreateOrder(dto);
            log.info("订单创建成功，orderId={}, orderNo={}", order.getId(), order.getOrderNo());
            return order;
        } catch (Exception e) {
            log.error("订单创建失败，userId={}, skuId={}", dto.getUserId(), dto.getSkuId(), e);
            throw e;
        }
    }
}
```

## 异常日志

- 打印异常时必须传入异常对象，保留堆栈。
- 不要只打印 `e.getMessage()`。
- 同一个异常不要在多层重复打印，避免日志噪音。
- 业务异常是否打印 ERROR 取决于是否需要人工介入。

## 敏感信息

日志中禁止输出：

- 密码、验证码、Token、Session ID
- 身份证、银行卡、完整手机号、完整邮箱
- 密钥、证书、数据库连接串
- 第三方接口完整签名参数

## TraceId

- 每个请求必须有 traceId。
- traceId 必须贯穿 Controller、Service、Mapper、外部调用和异步任务。
- 异步任务需要显式传递 MDC 上下文。
