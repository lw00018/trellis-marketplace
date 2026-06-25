# Model 类型规范

本文件覆盖 `src/model/{moduleName}.ts` 的类型位置、命名、导入和注释要求。

## 适用范围

以下类型默认放在 `src/model/{moduleName}.ts`：

- API 请求参数、响应项、详情、DTO、分页结果
- 页面表单、筛选项、下拉选项等可复用业务类型
- Store、composable、多个页面共享的业务类型

组件内部 Props / Emits 放在组件目录的 `interface.ts`，不放入 `src/model`。

## 固定位置

```txt
src/
├── api/
│   └── xxx.api.ts
├── model/
│   └── xxx.ts
├── store/
│   └── xxx.ts
└── views/
    └── xxx/
        ├── list.vue
        ├── add.vue                 ← 独立页面模式可选
        └── edit.vue                ← 独立页面模式可选
```

## 命名规范

| 类型 | 命名 | 说明 |
| --- | --- | --- |
| 列表项 | `XxxItem` | 列表表格的一行数据 |
| 详情 | `XxxDetail` | 详情接口返回数据 |
| 列表查询参数 | `XxxListParams` | 查询、筛选、分页参数 |
| 新增 DTO | `XxxCreateDTO` | 新增请求体 |
| 修改 DTO | `XxxUpdateDTO` | 修改请求体，通常包含 `id` |
| 下拉选项 | `XxxOption` | Select / TreeSelect 等选项 |
| 表单数据 | `XxxFormData` | 页面或模块级表单模型 |
| 分页结果 | `XxxListResult` | 列表分页响应结构 |

## 标准模板

```ts
/**
 * @description: Xxx 列表项
 * @param { string } id 主键 ID
 * @param { string } name 名称
 * @param { 'active' | 'inactive' } status 状态
 */
export interface XxxItem {
  id: string
  name: string
  status: 'active' | 'inactive'
}

/**
 * @description: Xxx 列表查询参数
 * @param { string } name 名称
 * @param { 'active' | 'inactive' } status 状态
 * @param { number } pageNum 页码
 * @param { number } pageSize 每页条数
 */
export interface XxxListParams {
  name?: string
  status?: 'active' | 'inactive'
  pageNum?: number
  pageSize?: number
}

/**
 * @description: Xxx 分页结果
 * @param { XxxItem[] } records 列表数据
 * @param { number } total 总数
 */
export interface XxxListResult {
  records: XxxItem[]
  total: number
}

/**
 * @description: Xxx 详情
 */
export interface XxxDetail extends XxxItem {
  description?: string
  createTime?: string
}

/**
 * @description: 新增 Xxx 参数
 */
export interface XxxCreateDTO {
  name: string
  status: 'active' | 'inactive'
  description?: string
}

/**
 * @description: 修改 Xxx 参数
 * @param { string } id 主键 ID
 */
export interface XxxUpdateDTO extends XxxCreateDTO {
  id: string
}
```

## 导入规则

跨文件引用 model 类型必须使用 `import type`：

```ts
import type { XxxItem, XxxListParams, XxxCreateDTO } from '@/model/xxx'
```

API、Vue 页面、Store、composable 都不得用普通 `import` 引入纯类型。

## 禁止项

- 禁止在 `src/api/*.api.ts` 中定义 `XxxItem`、`XxxListParams`、`XxxCreateDTO` 等业务类型。
- 禁止在 `.vue` 文件中定义可复用业务类型。
- 禁止把 API 请求体类型散落到 Store、composable 或页面文件。
- 禁止用 `any` 代替暂未确认的业务字段；不得已时使用 `MISSING_TYPE` 并补充中文说明。
