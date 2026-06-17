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
- DO、DTO、VO、BO、AO 等后缀必须语义一致。

## 可读性

- 方法只做一层抽象，不混合协议、业务、持久化细节。
- 条件复杂时提取有语义的方法。
- 注释解释原因和约束，不重复代码。
- TODO 必须带负责人或问题编号。

## Lombok

- 项目使用 Lombok，常用注解：`@Data`、`@Getter`/`@Setter`、`@Builder`、`@NoArgsConstructor`、`@AllArgsConstructor`、`@Slf4j`。
- 继承 `BaseEntity` / `BaseVO` / `BaseDTO` 的类使用 `@EqualsAndHashCode(callSuper = true)`。
- 禁止使用 `@Value`（不可变类用 `record` 替代）和 `@SneakyThrows`。

## 质量工具

- IDE 安装 Alibaba Java Coding Guidelines 插件。
- CI 接入 Checkstyle、Spotless、PMD 或 SonarQube。
- 强制规则失败必须阻断合并。
