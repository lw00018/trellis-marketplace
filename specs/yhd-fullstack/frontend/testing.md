# 单元测试规范

本文件覆盖前端 Vue3 工程的 Jest 单元测试约定。

## 技术栈

当前模板项目使用 Jest：

- `jest`
- `vue-jest`
- `ts-jest`
- `babel-jest`
- `jest-transform-stub`
- `jest-serializer-vue`
- `@vue/test-utils`

不要在当前模板项目中混用 Vitest API。测试文件使用 `describe`、`it`、`expect`、`jest.*`。

## 文件位置

`jest.config.js` 的匹配规则为：

```js
testMatch: ['**/tests/unit/**/*.spec.ts']
```

测试文件必须放在 `tests/unit/` 下：

```txt
tests/
└── unit/
    ├── example.spec.ts
    └── xxx/
        └── list.spec.ts
```

## 路径与转换

- `@/xxx` 映射到 `src/xxx`。
- `.vue` 文件由 `vue-jest` 转换。
- `.ts` / `.tsx` 文件由 `ts-jest` 转换。
- 样式、图片、字体等静态资源由 `jest-transform-stub` 转换。

## 组件测试

组件测试优先使用 `@vue/test-utils`：

```ts
import { shallowMount } from '@vue/test-utils'
import XxxComponent from '@/components/XxxComponent'

describe('XxxComponent', () => {
  it('正确渲染传入内容', () => {
    const wrapper = shallowMount(XxxComponent, {
      props: { title: '标题' },
    })

    expect(wrapper.text()).toContain('标题')
  })
})
```

## jsdom

`jest.config.js` 没有全局开启 `jsdom`。需要 DOM 环境的测试，在测试文件顶部按需声明：

```ts
/**
 * @jest-environment jsdom
 */
```

## 禁止项

- 禁止使用 `vi.*`、`vitest` 导入或 Vitest 专属 API。
- 禁止把测试放到 `tests/{module}/`、`src/**/__tests__/` 或其他不匹配 `testMatch` 的目录。
- 禁止为了让测试通过降低断言质量或改写实现能力。
- 禁止修改 `jest.config.js` 只为适配单个测试文件路径。
