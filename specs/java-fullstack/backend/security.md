# 安全规范

## 输入安全

- 所有外部输入必须校验，包括请求体、路径参数、查询参数、Header、Cookie、文件名。
- 前端传入的字段名、排序字段、回调地址、重定向地址必须经过白名单映射。
- 禁止信任客户端传入的用户 ID、角色、租户 ID、价格、状态等敏感业务字段。

## SQL 与持久化安全

- MyBatis XML 必须使用参数绑定，禁止字符串拼接 SQL。
- MyBatis-Plus Wrapper 禁止接收未校验的 SQL 片段。
- 更新和删除必须有明确条件。
- 多租户系统必须在数据访问层强制注入租户约束。

## 敏感数据

- 密码必须使用强哈希算法存储，例如 BCrypt、Argon2。
- Token、密钥、证书、数据库密码不得提交到仓库。
- 日志、异常、审计和响应中不得输出密码、Token、身份证、银行卡、手机号完整值。
- 敏感字段传输必须使用 HTTPS。

## 响应脱敏

- 脱敏注解统一引入 `com.yhd.common.sensitive` 包：
  - `com.yhd.common.sensitive.Sensitive`
  - `com.yhd.common.sensitive.SensitiveTypeEnum`
- `@Sensitive` 只标注在 ResponseVO 的 `String` 字段上，禁止标注在 Entity、DTO、RequestVO 或方法参数上。
- 优先使用语义明确的枚举类型，不使用 `CUSTOMER`；只有内置类型无法满足时才使用 `CUSTOMER` 并显式配置 `prefixNoMaskLen`、`suffixNoMaskLen`、`maskStr`。
- 脱敏只在 Jackson 序列化阶段生效，不影响数据库存储和内部业务逻辑。

### 强制脱敏字段

| 字段 | 推荐类型 |
| --- | --- |
| 手机号 | `MOBILE_PHONE` |
| 身份证号 | `ID_CARD` |
| 银行卡号 | `BANK_CARD` |
| 中文姓名 | `CHINESE_NAME` |
| 邮箱 | `EMAIL` |
| 密码 | `PASSWORD` |
| 密钥、Token、Secret | `KEY` |
| 详细地址 | `ADDRESS` |

### 脱敏效果

| `SensitiveTypeEnum` | 适用字段 | 脱敏效果示例 |
| --- | --- | --- |
| `CUSTOMER` | 自定义规则 | 按 `prefixNoMaskLen`、`suffixNoMaskLen`、`maskStr` 配置 |
| `CHINESE_NAME` | 中文姓名 | `刘*华`、`徐*` |
| `ID_CARD` | 身份证号 | `110110********1234` |
| `FIXED_PHONE` | 座机号 | `****1234` |
| `MOBILE_PHONE` | 手机号 | `176****1234` |
| `ADDRESS` | 地址 | `北京********` |
| `EMAIL` | 电子邮件 | `s*****o@xx.com` |
| `BANK_CARD` | 银行卡 | `622202************1234` |
| `PASSWORD` | 密码 | 永远 `******`，与长度无关 |
| `KEY` | 密钥 | 除最后三位外全部 `***` |

### 使用示例

```java
import com.yhd.common.sensitive.Sensitive;
import com.yhd.common.sensitive.SensitiveTypeEnum;

@Schema(description = "用户详情响应")
public class UserDetailResponseVO extends BaseVO {

    @Schema(description = "用户姓名")
    @Sensitive(type = SensitiveTypeEnum.CHINESE_NAME)
    private String name;

    @Schema(description = "手机号")
    @Sensitive(type = SensitiveTypeEnum.MOBILE_PHONE)
    private String mobile;

    @Schema(description = "身份证号")
    @Sensitive(type = SensitiveTypeEnum.ID_CARD)
    private String idCard;

    @Schema(description = "邮箱")
    @Sensitive(type = SensitiveTypeEnum.EMAIL)
    private String email;

    @Schema(description = "银行卡号")
    @Sensitive(type = SensitiveTypeEnum.BANK_CARD)
    private String bankCard;

    @Schema(description = "详细地址")
    @Sensitive(type = SensitiveTypeEnum.ADDRESS)
    private String address;
}
```

自定义脱敏：

```java
import com.yhd.common.sensitive.Sensitive;
import com.yhd.common.sensitive.SensitiveTypeEnum;

@Sensitive(
    type = SensitiveTypeEnum.CUSTOMER,
    prefixNoMaskLen = 4,
    suffixNoMaskLen = 4,
    maskStr = "*"
)
private String orderNo;
```

### 禁止事项

- 禁止在日志中输出脱敏前的原始值。
- 禁止为了调试临时移除 `@Sensitive` 注解并提交。
- 禁止在 RequestVO 中使用 `@Sensitive`，入参不脱敏。
- 禁止依赖 `@Sensitive` 作为唯一安全手段，数据库和日志仍需独立脱敏。

## 文件上传

- 校验文件大小、扩展名、MIME 类型和实际内容。
- 文件名必须重新生成，不能直接使用用户文件名作为存储路径。
- 上传文件不得存放在可执行目录。
- 图片和文档处理必须防范压缩炸弹和解析漏洞。

## OWASP 风险

- 防注入：SQL、NoSQL、命令、模板、表达式都必须参数化或白名单。
- 防越权：每个业务操作都必须检查资源归属和权限。
- 防敏感数据泄露：错误响应不能暴露堆栈和内部实现。
- 防安全配置错误：生产环境关闭调试端点和默认账号。

## 安全审查问题

- 这个接口是否能被未授权用户调用。
- 用户是否能访问不属于自己的资源。
- 是否存在可被用户控制的 SQL、路径、URL 或命令。
- 日志和响应是否泄露敏感信息。
- 密钥是否来自安全配置，而不是代码或配置文件提交。
