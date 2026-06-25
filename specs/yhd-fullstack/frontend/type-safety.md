# TypeScript 规范

本文件覆盖 TypeScript 基础约束、类型声明存放位置、命名规范和编译配置。Model 专题见 [models.md](./models.md)，注释专题见 [comments.md](./comments.md)。

## 类型声明存放位置

**接口和页面相关类型** → `src/model/{moduleName}.ts`

- API 接口类型（Item、DTO、Params、Detail）
- 页面使用的相关数据类型
- 多个页面共享的业务类型

**组件相关类型** → 组件目录下 `interface.ts`

- 组件 Props 接口
- 组件 Emits 接口
- 组件内部使用的类型定义

| 类型范围 | 存放位置 | 示例 |
| --- | --- | --- |
| API 接口类型 | `src/model/xxx.ts` | `XxxItem`、`XxxCreateDTO`、`XxxListParams` |
| 页面使用类型 | `src/model/xxx.ts` | `XxxFormData`、`XxxOption` |
| 组件 Props/Emits | `ComponentName/interface.ts` | `ComponentNameProps`、`ComponentNameEmits` |
| 全局枚举常量 | `src/enums/` | `appEnum.ts`、`cacheEnum.ts` |
| 全局类型定义 | `/types/` (根目录) | `app.config.ts`、`env.config.ts` |

## 编译配置

当前模板项目 `tsconfig.json` 的关键约束：

- `strict: true`
- `noImplicitAny: true`
- `noUnusedLocals: true`
- `noUnusedParameters: true`
- `noFallthroughCasesInSwitch: true`
- `module` / `target`: ESNext
- `moduleResolution`: Node
- `types`: `node`、`jest`
- 路径别名：`@/*` → `src/*`，`~/*` → 项目根

## 类型命名规范

| 类型 | 命名格式 | 示例 |
| --- | --- | --- |
| 列表查询参数 | `XxxListParams` | `OrderListParams` |
| 新增 DTO | `XxxCreateDTO` | `OrderCreateDTO` |
| 修改 DTO | `XxxUpdateDTO` | `OrderUpdateDTO` |
| 列表项 | `XxxItem` | `OrderItem` |
| 详情 | `XxxDetail` | `OrderDetail` |

## 专题规则

- Model 类型位置、命名和 `import type` 规则见 [models.md](./models.md)。
- 中文注释、JSDoc 格式和注释密度见 [comments.md](./comments.md)。

## 禁止项

- **禁止**无差别使用 `any` 和 `unknown`
- 不得已时使用 `MISSING_TYPE` 代替 raw `any`（grep 可追踪）
- Vue 组件内的变量、函数参数、ref、reactive 等都应有明确类型标注
- **禁止**手动编辑 `/types/auto-imports.d.ts`、`/types/components.d.ts` 和 `.eslintrc-auto-import.json`
- 未使用变量和未使用参数会触发 TypeScript 错误；不要通过改低 `tsconfig.json` 严格度绕过
