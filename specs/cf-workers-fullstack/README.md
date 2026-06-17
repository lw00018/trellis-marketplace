# Cloudflare Workers 全栈开发指南

面向使用 Hono 框架、Drizzle ORM 和 Turso 数据库的 Cloudflare Workers 全栈应用的生产开发指南。

## 结构

### [后端](./backend/index.md)

Cloudflare Workers + Hono + Drizzle ORM 后端开发模式：

- [API 模块](./backend/api-module.md)
- [API 模式](./backend/api-patterns.md)
- [数据库](./backend/database.md)
- [环境](./backend/environment.md)
- [错误与日志](./backend/error-logging.md)
- [Hono 框架](./backend/hono-framework.md)
- [质量检查清单](./backend/quality.md)
- [安全](./backend/security.md)
- [存储](./backend/storage.md)
- [类型安全](./backend/type-safety.md)

### [前端](./frontend/index.md)

React Router v7 + Vite + TailwindCSS 前端开发模式：

- [身份认证](./frontend/authentication.md)
- [组件](./frontend/components.md)
- [目录结构](./frontend/directory-structure.md)
- [Hooks](./frontend/hooks.md)
- [质量检查清单](./frontend/quality.md)
- [类型安全](./frontend/type-safety.md)
- [示例：前端设计](./frontend/examples/frontend-design/)

### [共享](./shared/index.md)

横切关注点：

- [代码质量](./shared/code-quality.md)
- [依赖版本](./shared/dependency-versions.md)
- [TypeScript 约定](./shared/typescript.md)
- [时间戳](./shared/timestamp.md)

### [指南](./guides/index.md)

开发思考指南和设计模式：

- [OAuth 授权同意流程](./guides/oauth-consent-flow.md)
- [Serverless 连接指南](./guides/serverless-connection-guide.md)

### [常见问题 / 陷阱](./big-question/index.md)

Cloudflare Workers 应用的常见问题和解决方案：

- [Workers Node.js 兼容性](./big-question/workers-nodejs-compat.md)
- [跨层契约](./big-question/cross-layer-contract.md)
- [CSS 调试思考指南](./big-question/css-debugging-thinking-guide.md)
- [环境配置](./big-question/env-configuration.md)
- [系统约束](./big-question/system-constraints.md)

## 技术栈

- **运行时**：Cloudflare Workers（Wrangler v4+）
- **后端**：Hono、Drizzle ORM、Turso（libSQL/SQLite）
- **前端**：React Router v7、Vite、TailwindCSS、shadcn/ui
- **认证**：Better Auth
- **存储**：Cloudflare KV、R2
- **构建**：Vite + @cloudflare/vite-plugin
- **语言**：全项目使用 TypeScript

## 用法

这些指南可用作：

1. **新项目模板** - 为新的 Cloudflare Workers 项目复制整套结构
2. **参考文档** - 实现功能时查阅具体指南
3. **代码审查清单** - 根据既定模式验证实现
4. **入职材料** - 帮助新开发者理解项目约定
