# Jackson 序列化循环引用

## 现象

- 返回对象时栈溢出。
- JSON 响应包含巨大嵌套对象。
- 序列化触发懒加载和额外查询。

## 根因

Entity 存在双向关联，Jackson 序列化时沿对象图无限展开。

## 解决方案

- API 返回 ResponseVO，不直接返回 Entity 或 DTO。
- 响应对象只包含当前接口需要的字段。
- 必要时使用 Jackson 注解控制序列化，但不要作为主要架构方案。

## 预防

- Entity 不跨越 Controller 边界。
- 转换层显式选择响应字段。
