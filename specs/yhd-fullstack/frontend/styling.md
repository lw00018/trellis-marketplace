# 样式与代码格式化规范

本文件覆盖样式语言、BEM 命名、Less 变量、Prettier 配置、pre-commit。

## 样式语言

使用 `<style lang="less" scoped>`，优先级：**Tailwind > Less 变量/混合**。

模板项目在 `vite.config.ts` 中为 Less 注入：

- `src/assets/css/variables.less`：变量和层级定义。
- `src/assets/css/index.less`：统一入口，继续引入 `mixin.less`、`style.less`、`antd.less`、`transition.less`。

业务组件可直接使用 `@primary-color`、`@error-color`、`@progress-z-index` 等 Less 变量，不要在组件内重复硬编码同类基础色和层级。

## BEM 命名规范

使用 **BEM (Block Element Modifier)** 命名规范：

```
.block {}
.block__element {}
.block--modifier {}
.block__element--modifier {}
```

| 部分 | 分隔符 | 用途 | 示例 |
| --- | --- | --- | --- |
| **Block** | — | 独立的有意义组件 | `.user-card`、`.search-form` |
| **Element** | `__` (双下划线) | Block 的部分 | `.user-card__avatar`、`.search-form__input` |
| **Modifier** | `--` (双连字符) | 状态或变体 | `.user-card--active`、`.user-card__avatar--large` |

使用 `&` 父选择器嵌套：

```less
.user-list {
  // Element: 使用 &__ 前缀
  &__header {
    /* ... */
  }

  &__table {
    /* ... */

    // Element 的 Modifier
    &__action {
      /* ... */
      // Modifier
      &--danger {
        color: @error-color;
      }
    }
  }

  // Block 的 Modifier
  &--loading {
    opacity: 0.5;
  }
}
```

## 命名规则

| 规则 | 正确 | 错误 |
| --- | --- | --- |
| Block: kebab-case | `.user-card` | `.userCard`、`.UserCard` |
| Element: 双下划线 | `.user-card__avatar` | `.user-card-avatar` |
| Modifier: 双连字符 | `.user-card--active` | `.user-card.active` |
| 最大嵌套层级 | 1 层 element | `.block__element__sub-element` |
| 状态类 | `.block--active` | `.block.is-active` |

## 页面根元素

页面组件根元素**必须**使用 `page-wrapper-content` 类名。

## Less 变量（`src/assets/css/variables.less`）

| 变量 | 默认值 | 说明 |
| --- | --- | --- |
| `@primary-color` | `#1db3c9` | 主色 |
| `@primary-color-hover` | lighter 20% | 主色 hover |
| `@primary-color-focus` | darker 10% | 主色 focus |
| `@link-color` | `#1356ef` | 链接色 |
| `@success-color` | `#1bc27c` | 成功色 |
| `@warning-color` | `#ff7700` | 警告色 |
| `@error-color` | `#f61c1c` | 错误色 |
| `@danger-color` | `#f96868` | 危险色 |
| `@text-color` | `#333333` | 主要文字 |
| `@text-color-secondary` | `#999999` | 次要文字/占位符 |
| `@disabled-color` | `#cccccc` | 禁用色 |
| `@border-color-base` | `#d9d9d9` | 边框色 |
| `@divider-color` | `#f0f0f0` | 分割线 |
| `@component-background` | `#ffffff` | 组件背景 |
| `@table-header-bg` | `#f5f5f5` | 表头背景 |
| `@font-size-base` | `14px` | 基础字体大小 |
| `@border-radius-base` | `2px` | 组件/浮层圆角 |
| `@box-shadow-base` | `0 2px 8px rgba(0, 0, 0, 0.15)` | 浮层阴影 |
| `@table-selected-row-bg` | `#f5f7fa` | 表格选中 |
| `@side-menu-bg` | `#f8f8f8` | 侧边栏背景 |
| `@progress-z-index` | `1085` | 页面进度层级 |
| `@sideBar-z-index` | `1070` | 侧边栏层级 |
| `@tabs-z-index` | `1060` | 标签栏层级 |

## Tailwind 配置

- 间距单位：**px**（非 rem）
- preflight: **disabled**

## Prettier 配置（强制）

```json
{
  "printWidth": 100,
  "singleQuote": true,
  "bracketSpacing": true,
  "tabWidth": 2,
  "semi": false,
  "arrowParens": "always",
  "endOfLine": "lf",
  "trailingComma": "es5"
}
```

## ESLint 与 Pre-commit

- ESLint 继承 `@yhdfe/vue` 和 `.eslintrc-auto-import.json`。
- `package.json` 的 `lint` 脚本为 `eslint --ext .js,.ts,.jsx,.tsx,.vue src --fix`。
- `lint-staged` 对 `*.{vue,js,ts,tsx}` 执行 `eslint --fix`。
- 不要为了通过检查调低 `.eslintrc.js`、`.prettierrc` 或 `tsconfig.json` 严格度。
