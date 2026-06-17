# 数据库与 MyBatis-Plus 规范

## 实体映射

- 表名不符合驼峰映射时必须显式使用 `@TableName`。
- 主键字段不是 `id` 时必须显式使用 `@TableId`。
- 非表字段必须使用 `@TableField(exist = false)` 或 `transient`。
- 审计字段统一命名，例如 `created_date`、`updated_date`、`created_by`、`updated_by`。
- 逻辑删除字段统一命名为 `del_flag`。

## Mapper 配置

```yaml
mybatis-plus:
  mapper-locations: classpath*:/mapper/**/*.xml
  configuration:
    map-underscore-to-camel-case: true
```

- Mapper 扫描统一使用 `@MapperScan`。
- 多模块项目必须使用 `classpath*:`，避免依赖包中的 XML 无法加载。
- XML namespace 必须与 Mapper 全限定名一致。

## Wrapper 使用

- 优先使用 `LambdaQueryWrapper` 和 `LambdaUpdateWrapper`。
- 禁止使用前端传入的字段名直接拼接列名。
- 动态条件使用 boolean 参数控制，不手写字符串拼接。
- 排序字段必须通过白名单映射。

## SQL 安全

- 禁止把前端传入 SQL 片段、列名、表名、排序表达式。
- 自定义 SQL 必须使用参数绑定。
- 批量更新和删除必须有明确条件。
- 必须启用防全表更新删除策略或在代码审查中强制检查。

## 索引

- 高频查询条件、关联字段、唯一约束必须设计索引。
- 联合索引遵循最左前缀原则。
- 禁止为了“可能会查”创建大量无依据索引。
- 慢 SQL 必须结合执行计划分析，而不是只靠猜测加索引。

## 数据库迁移

- 生产项目必须使用 Flyway 或 Liquibase 管理结构变更。
- 数据库结构变更必须可回滚或有明确修复脚本。
- 修改字段语义前必须搜索所有读写路径。
- 结构变更流程参考 [数据库结构变更指南](../guides/db-schema-change-guide.md)。

## 大数据量处理

- 普通列表查询必须分页。
- 批处理使用分批分页、游标或流式查询。
- 禁止一次性加载全表到内存。
- 批量写入必须控制批大小和事务范围。
