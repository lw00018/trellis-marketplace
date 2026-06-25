# REST API 模式

## 路径设计

- API 路径使用小写 kebab-case 和名词复数：`/api/v1/users`。
- 子资源表达从属关系：`/api/v1/users/{userId}/roles`。
- 动作用 HTTP 方法表达，避免 `/createUser`、`/deleteUser`。
- 非 CRUD 业务动作可以使用语义子资源：`POST /api/v1/orders/{id}/cancel`。

## HTTP 方法

| 方法 | 用途 |
| --- | --- |
| GET | 查询资源，不产生副作用 |
| POST | 创建资源或提交命令 |
| PUT | 全量替换资源 |
| PATCH | 局部更新资源 |
| DELETE | 删除或停用资源 |

## 响应结构

统一响应使用 `com.yhd.common.pojo.vo.BusinessResponse<T>`，结构如下：

```json
{
  "rt_code": 0,
  "rt_msg": "success",
  "data": {}
}
```

- `rt_code`：响应状态码，`0` 表示成功。
- `rt_msg`：响应消息，成功时为 `"success"`。
- `data`：响应数据，泛型。

### 常用状态码常量

| 常量 | 值 | 含义 |
| --- | --- | --- |
| `RESPONSE_OK` | `0` | 成功 |
| `RESPONSE_PARA_ERROR` | `400000` | 参数错误 |
| `RESPONSE_NO_RIGHT` | `403000` | 无权限 |
| `RESPONSE_NO_DATA` | `204000` | 无数据 |
| `RESPONSE_ERROR` | `500000` | 服务端错误（默认值） |

### 构造方式

```java
// 成功
BusinessResponse.ok(data);

// 失败（仅消息，默认 500000）
BusinessResponse.fail("错误描述");

// 失败（指定状态码和消息）
BusinessResponse.fail(400000, "参数非法");

// 失败（指定状态码、消息和数据）
BusinessResponse.fail(code, msg, data);
```

### 判断成功

```java
response.success()  // rt_code == 0
```


## HTTP 状态码与业务码

业务码通过 `rt_code` 返回，HTTP 状态码仍遵循 REST 规范：

| HTTP 状态码 | 业务码常量 | rt_code | 含义 |
| --- | --- | --- | --- |
| `200` | `RESPONSE_OK` | `0` | 查询或同步命令成功 |
| `204` | `RESPONSE_NO_DATA` | `204000` | 成功但无数据 |
| `400` | `RESPONSE_PARA_ERROR` | `400000` | 请求参数非法 |
| `403` | `RESPONSE_NO_RIGHT` | `403000` | 已认证但无权限 |
| `409` | — | — | 资源冲突或状态冲突 |
| `422` | — | — | 业务语义校验失败 |
| `500` | `RESPONSE_ERROR` | `500000` | 未预期服务端错误 |

- `201` 资源创建成功时 `rt_code` 仍为 `0`，HTTP 状态码为 `201`。
- 自定义业务码应避免与上述常量值冲突，建议按 `4XX000` / `5XX000` 区间扩展。

## 分页

分页请求必须限制最大页大小：

```json
{
  "pageNum": 1,
  "pageSize": 20
}
```

- `pageNum` 从 1 开始。
- `pageSize` 必须有默认值和最大值。
- 排序字段必须由后端白名单映射，不能直接使用前端字段名拼接 SQL。

## API 兼容性

- 删除字段、修改字段语义、修改枚举含义属于破坏性变更。
- 新增可选字段通常兼容。
- 公开 API 必须保留版本号，例如 `/api/v1`。
- 版本升级参考 [API 版本管理指南](../guides/api-versioning-guide.md)。
