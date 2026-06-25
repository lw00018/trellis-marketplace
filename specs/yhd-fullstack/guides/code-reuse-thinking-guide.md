# 代码复用思考指南

本指南用于在新增工具函数、Hook、组件、类型、常量或业务逻辑前判断是否已有可复用能力，避免重复实现和目录膨胀。

## 核心原则

在写新代码前先搜索：

```bash
rg "关键字段|相似函数|相似组件|相似接口" src tests
```

如果已有能力满足 80% 场景，优先复用或轻量扩展；只有确实没有合适位置时才新增。

## 新增前检查

### 1. 工具函数

- [ ] VueUse 是否已有对应组合式函数？优先使用 `@vueuse/core`。
- [ ] `src/utils/` 是否已有缓存、路由、进度条、浏览器检测或复制过滤能力？
- [ ] 是否只是当前组件内部使用的纯函数？单文件内维护即可。
- [ ] 是否跨多个模块复用？放到 `src/utils/` 并补 Jest 单元测试。

禁止新增重复的缓存、路由扁平化、进度条、浏览器检测工具。缓存必须使用 `localCache` / `sessionCache`。

### 2. Hooks / Composable

- [ ] 导航、刷新、标签页是否可用 `src/hooks/web/usePage.ts` 或 `src/hooks/web/useTabs.ts`？
- [ ] 布局、loading、菜单宽度、Tabs 高度是否可用 `src/hooks/setting/useRootSetting.ts`？
- [ ] 当前逻辑是否只服务某个复杂组件？放到组件目录 `composable/`。
- [ ] 是否跨页面复用响应式流程？放到 `src/hooks/` 或业务 composable。

`composable/` 只放响应式状态流程、派生状态、监听副作用、生命周期副作用和围绕状态的操作函数，不放纯函数、静态配置、API 定义或类型定义。

### 3. 组件

- [ ] 是否已有项目公共组件或 Ant Design Vue / YUI 组件能满足？
- [ ] 是否只是当前页面私有展示块？放到页面或组件私有 `components/`。
- [ ] 是否跨页面复用且具有稳定 API？放到 `src/components/ComponentName/`。
- [ ] 是否需要 `index.ts` 作为唯一出口，避免外部绕过内部文件？

复杂组件按以下结构扩展：

```text
ComponentName/
├── ComponentName.vue
├── index.ts
├── interface.ts
├── config.ts
├── context.ts
├── components/
└── composable/
```

不要为了单个常量、单个方法或少量配置创建多层目录。

### 4. 类型、枚举与静态配置

| 内容 | 推荐位置 |
| --- | --- |
| 组件 props / emits / slots / expose 类型 | 组件目录 `interface.ts` |
| 当前组件私有枚举、状态映射、表格列、默认表单值 | 组件目录 `config.ts` |
| API 入参和响应类型 | `src/model/` 或接口对应 model 文件 |
| 跨模块共享业务枚举 | 业务模块合适的 model / constants 文件 |
| 单文件内部临时常量 | 当前 `.vue` 或 `.ts` 文件内 |

不要把类型放进 `composable/`，不要在多个页面重复定义相同枚举或状态映射。

### 5. API 与请求

- [ ] 是否已有 `src/api/` 中的接口方法？
- [ ] 是否已有 upload URL 常量或分页查询封装？
- [ ] 是否能复用项目 request 的 loading、token、错误处理能力？
- [ ] 是否需要补充对应 `src/model/` 类型？

禁止在页面中创建新的 HTTP client，禁止为了单个接口修改 `src/request/index.ts`。

## 抽取判断

| 重复次数 | 建议 |
| --- | --- |
| 1 次 | 直接写在当前文件，保持简单 |
| 2 次 | 可先观察；如果语义稳定，可抽到组件私有或模块私有位置 |
| 3 次及以上 | 应抽取到合适的 `utils`、`hooks`、组件或 model |

抽取前先确认命名、参数、返回值和错误处理是否足够稳定；不要为了假想复用提前设计复杂抽象。

## 常见错误

- 页面内重复实现缓存读写，而不是使用 `localCache` / `sessionCache`。
- 多个页面各自写 `useGo`、刷新当前页或关闭标签页逻辑。
- 表格列、状态颜色和筛选项在多个文件重复硬编码。
- 同一个 API 响应类型在页面、Store、组件中重复定义。
- 把纯函数、静态配置或类型塞进 `composable/`。
- 为了一个页面需求改全局入口、插件或 request 基础设施。

## 完成前确认

- [ ] 已搜索同名或相似实现。
- [ ] 已优先复用 VueUse、项目 Hooks 和项目 Utils。
- [ ] 新增文件的位置符合职责边界。
- [ ] 跨模块复用的纯函数已补 Jest 单元测试。
- [ ] 没有新增与现有封装重复的全局能力。
