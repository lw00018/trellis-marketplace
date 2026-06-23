# JPA 懒加载边界

## 现象

- 返回响应或序列化对象时出现懒加载异常。
- 查询列表后访问关联对象触发大量 SQL。

## 根因

懒加载依赖持久化上下文。离开事务或 Session 后访问懒加载属性会失败，循环访问关联对象还可能造成 N+1 查询。

## 解决方案

- API 返回 ResponseVO，不直接返回 Entity 或 DTO。
- 查询时明确需要的关联数据。
- 对列表接口使用专门查询模型。
- 避免开启 Open Session In View 掩盖边界问题。

## 说明

本模板默认使用 MyBatis-Plus，但很多 Java 项目会混用 JPA。本问题用于提醒 ORM Entity 不应跨越 API 边界。
