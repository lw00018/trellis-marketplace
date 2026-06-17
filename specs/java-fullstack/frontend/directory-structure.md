# 前端目录结构

## 推荐结构

```text
src/
├── app/ 或 pages/
├── components/
├── features/
│   ├── user/
│   │   ├── api/
│   │   ├── components/
│   │   ├── hooks/
│   │   └── types/
├── lib/
├── styles/
└── types/
```

## 规则

- 业务代码优先按 `features/{domain}` 组织。
- 通用组件放 `components`，业务组件放对应 feature 内。
- API 调用必须集中在 `api` 目录，不在组件中直接散落请求 URL。
- 类型定义优先来自后端契约生成，不重复手写。
- `lib` 只放无业务状态的基础工具。

## 与后端对齐

- 前端 feature 名称应与后端领域模块保持一致。
- API 路径、错误码、枚举值必须来自共享契约或文档。
- 新增页面前先确认后端是否已有对应 DTO、VO 和分页结构。
