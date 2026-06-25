# API 规范

本文件覆盖 API 文件组织、函数命名、请求配置、类型来源和注释规范。

## 文件与命名

- 文件：`src/api/{moduleName}.api.ts`
- 函数名：`{moduleName}Api{Verb}{Object}`（camelCase）
- 每个导出函数必须有 JSDoc `@description:` 注释（冒号必需）。
- 文件内排序：Get → Create → Update → Delete → 其他操作。

## 类型来源

API 文件只定义请求函数，不定义业务 `type` / `interface`。

```ts
import request from '@/request'
import type { CustomResponse } from '@yhdfe-plugins/axios'
import type { XxxListParams, XxxListResult } from '@/model/xxx'
```

- `XxxItem`、`XxxDetail`、`XxxListParams`、`XxxCreateDTO`、`XxxUpdateDTO` 等类型必须来自 `src/model/{moduleName}.ts`。
- 纯类型必须用 `import type`。
- `CustomResponse` 类型必须从 `@yhdfe-plugins/axios` 导入，不得自定义同名类型。

## 17 个标准动词

| 动词 | 语义 | 示例 |
| --- | --- | --- |
| Get | 查询 | `orderApiGetList`、`orderApiGetDetail` |
| Create | 新增 | `orderApiCreate` |
| Update | 修改 | `orderApiUpdate` |
| Delete | 删除 | `orderApiDelete` |
| Save | 保存（新增 + 修改） | `orderApiSave` |
| Submit | 提交 | `orderApiSubmit` |
| Export | 导出 | `orderApiExport` |
| Import | 导入 | `orderApiImport` |
| Upload | 上传 | `orderApiUpload` |
| Download | 下载 | `orderApiDownload` |
| Check | 校验 | `orderApiCheck` |
| BatchDelete | 批量删除 | `orderApiBatchDelete` |
| Enable | 启用 | `orderApiEnable` |
| Disable | 禁用 | `orderApiDisable` |
| Cancel | 取消 | `orderApiCancel` |
| Approve | 审批通过 | `orderApiApprove` |
| Reject | 审批驳回 | `orderApiReject` |

## API 函数风格

```ts
/**
 * @description: 查询 Xxx 列表
 * @param { XxxListParams } params 列表查询参数
 * @return { Promise<CustomResponse<XxxListResult>> } 分页列表数据
 */
export const xxxApiGetList = (
  params: XxxListParams
): Promise<CustomResponse<XxxListResult>> => {
  return request.post('/xxx/list', params, { loading: true })
}
```

## 请求配置

`request` 来自 `@/request`，底层由 `src/request/index.ts` 基于 `@yhdfe-plugins/axios` 创建，并接入：

- `app.config.ts` 中的 `netConfig.baseURL`、`timeout`、`headerTokenName`
- `@yhdfe-plugins/portal-cms-vue` 的 `getToken`
- `@yhdfe-plugins/axios-adapter` 的错误消息适配
- `@/utils/progress` 的 loading 进度控制
- `ant-design-vue` 的 `message.error`

业务 API 只传当前接口需要覆盖的请求配置：

```ts
request.post(url, data, {
  loading: true,
  isNeedShowError: true,
  isNeedToken: true,
  responseType: 'blob',
})
```

上传 URL 可按模板项目方式导出常量：

```ts
export const xxxApiUploadUrl = '/apiXxx/upload'
```

## 错误处理

- 后端错误码必须映射为用户可理解提示。
- 未认证跳转登录，权限不足展示无权限页面或提示。
- 未预期错误展示通用提示并保留 traceId。
- 禁止把后端原始堆栈或 SQL 错误展示给用户。

## 分页和排序

- 前端使用 `pageNo` 和 `pageSize` 与后端保持一致。
- 排序字段使用后端允许的字段枚举，不传任意列名。
- 列表页必须处理空状态、加载中、错误和重试。

## 契约变更

- 修改 API 字段前必须更新共享契约和前端调用方。
- 删除字段或改变语义必须走版本管理流程。
- 前端不得依赖未公开的后端内部字段。

## 禁止项

- 禁止直接使用 `axios`，必须使用 `@/request`。
- 禁止修改 `src/request/index.ts` 适配单个业务接口。
- 禁止在 API 文件内定义业务类型。
- 禁止 `getOrderList`、`fetchXxx`、`addXxx`、`removeXxx`、`modifyXxx`、`queryXxx` 等非 `{moduleName}Api{Verb}{Object}` 命名。
- 禁止用普通 `import` 导入纯类型。
