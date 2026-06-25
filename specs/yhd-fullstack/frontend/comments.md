# 注释规范

本文件覆盖中文注释、JSDoc 格式、Vue 文件注释方式和注释密度。

## 基本原则

- 注释使用中文。
- 只注释业务含义、复杂规则、导出 API、model 类型字段、关键处理函数。
- 优先通过命名表达代码意图，不写解释代码"做什么"的废话注释。
- 不在注释中记录当前任务、临时改动原因、需求单号或调用方背景。

## JSDoc 格式

`@description:` 冒号必需。

```ts
/**
 * @description: 获取列表数据
 * @param { XxxListParams } params 列表查询参数
 * @return { Promise<CustomResponse<XxxListResult>> } 分页列表数据
 */
const fetchList = async (params: XxxListParams) => {
  // ...
}
```

规则：

- `@description:` 必填，用一句中文描述业务动作或业务含义。
- `@param { Type } name 说明`，类型必须放在花括号中。
- `@return { Type } 说明` 仅在返回值不显而易见时使用。
- 参数与说明之间保留一个空格即可，不额外堆叠空格。

## Vue 文件注释

```vue
<script setup lang="ts">
defineOptions({ name: 'XxxList' })

// 搜索表单
const searchForm = reactive<XxxListParams>({ name: '', status: undefined })

// 加载状态
const loading = ref(false)

/**
 * @description: 获取列表数据
 */
const fetchList = async () => {
  // ...
}
</script>
```

| 区域 | 注释方式 |
| --- | --- |
| `template` | 必要分区使用 `<!-- 搜索区域 -->`、`<!-- 数据表格 -->` |
| `script` 变量 / 状态 | 使用 `//` 行注释 |
| `script` 方法 | 使用 JSDoc `@description:` |
| `style` | 默认不写注释 |

## 按位置分级

| 位置 | 注释要求 |
| --- | --- |
| `src/model/` | 每个 interface / type 需要 JSDoc，字段用 `@param` 说明 |
| `src/api/*.api.ts` | 每个导出函数需要 `@description:`，有接口文档时补 `@link:` |
| `src/components/*/interface.ts` | Props / Emits 需要 JSDoc |
| `src/views/**/*.vue` | 关键状态用 `//`，主要业务方法用 JSDoc |
| `src/store/` | state / computed 用 `//`，actions 用 JSDoc |
| `src/hooks/`、`src/utils/` | 导出函数使用 JSDoc |

## 禁止项

- 禁止英文泛化注释，如 `// data`、`// methods`、`// TODO fix later`。
- 禁止解释代码字面行为，如 `// 设置 loading 为 true`。
- 禁止为每一行简单赋值都添加注释。
- 禁止把需求背景、任务说明、临时方案写入代码注释。
- 禁止省略 `@description:` 的冒号。
