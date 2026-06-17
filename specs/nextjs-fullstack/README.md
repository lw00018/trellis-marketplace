# Next.js 全栈开发指南

面向使用 oRPC API 层和 PostgreSQL 的生产级 Next.js 应用的通用开发指南。

## 结构

### [前端](./frontend/index.md)

React 19 + Next.js 15 App Router 前端开发模式：

- [目录结构](./frontend/directory-structure.md)
- [组件](./frontend/components.md)
- [状态管理](./frontend/state-management.md)
- [Hooks](./frontend/hooks.md)
- [API 集成](./frontend/api-integration.md)
- [oRPC 用法](./frontend/orpc-usage.md)
- [身份认证](./frontend/authentication.md)
- [AI SDK 集成](./frontend/ai-sdk-integration.md)
- [CSS 与布局](./frontend/css-layout.md)
- [类型安全](./frontend/type-safety.md)
- [质量检查清单](./frontend/quality.md)

### [后端](./backend/index.md)

oRPC + Drizzle ORM 后端开发模式：

- [目录结构](./backend/directory-structure.md)
- [oRPC 用法](./backend/orpc-usage.md)
- [身份认证](./backend/authentication.md)
- [数据库](./backend/database.md)
- [AI SDK 集成](./backend/ai-sdk-integration.md)
- [日志](./backend/logging.md)
- [性能](./backend/performance.md)
- [类型安全](./backend/type-safety.md)
- [质量检查清单](./backend/quality.md)

### [共享](./shared/index.md)

横切关注点：

- [依赖](./shared/dependencies.md)
- [代码质量](./shared/code-quality.md)
- [TypeScript 约定](./shared/typescript.md)

### [指南](./guides/index.md)

开发思考指南：

- [实现前检查清单](./guides/pre-implementation-checklist.md)
- [跨层思考指南](./guides/cross-layer-thinking-guide.md)

### [常见问题 / 陷阱](./big-question/index.md)

常见问题和解决方案：

- [PostgreSQL JSON 与 JSONB](./big-question/postgres-json-jsonb.md)
- [WebKit 点击高亮](./big-question/webkit-tap-highlight.md)
- [Sentry 与 next-intl 冲突](./big-question/sentry-nextintl-conflict.md)
- [Turbopack 与 Webpack Flexbox](./big-question/turbopack-webpack-flexbox.md)

## 技术栈

- **前端**：Next.js 15、React 19、TailwindCSS 4、Radix UI、React Query
- **后端**：oRPC、Drizzle ORM、PostgreSQL、better-auth
- **AI**：Vercel AI SDK（多提供商）
- **实时通信**：Ably / WebSocket / SSE
- **构建**：Turborepo + pnpm（monorepo）
- **监控**：Sentry + OpenTelemetry

## 用法

这些指南可用作：

1. **新项目模板** - 为新的 Next.js 项目复制整套结构
2. **参考文档** - 实现功能时查阅具体指南
3. **代码审查清单** - 根据既定模式验证实现
4. **入职材料** - 帮助新开发者理解项目约定
