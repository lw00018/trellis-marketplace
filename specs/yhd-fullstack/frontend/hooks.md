# Hooks 规范

## 职责

- Hooks 用于复用状态逻辑、副作用和 API 查询封装。
- 请求类 Hooks 命名使用 `useUsersQuery`、`useCreateUserMutation`。
- UI 状态 Hooks 命名使用 `useDialogState`、`useSelection`。

## 规则

- Hooks 不得条件调用。
- Hooks 内副作用必须声明完整依赖。
- 请求 Hooks 必须统一处理加载、错误、重试和缓存失效。
- 不要把一次性页面逻辑过早抽成通用 Hooks。

## 后端协作

- Mutation 成功后必须明确刷新哪些查询缓存。
- 后端错误码必须在 Hooks 或 API 层统一转换。
- 表单提交 Hooks 必须处理重复提交。
