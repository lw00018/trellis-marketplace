# Electron 开发指南

从生产级 Electron 项目中提炼出的通用开发指南。

## 结构

### [前端](./frontend/index.md)

React + TypeScript 前端开发模式：

- [目录结构](./frontend/directory-structure.md)
- [组件](./frontend/components.md)
- [状态管理](./frontend/state-management.md)
- [Hooks](./frontend/hooks.md)
- [IPC 通信](./frontend/ipc-electron.md)
- [CSS 设计](./frontend/css-design.md)
- [类型安全](./frontend/type-safety.md)
- [React 陷阱](./frontend/react-pitfalls.md)
- [Electron 浏览器 API 限制](./frontend/electron-browser-api-restrictions.md)

### [后端](./backend/index.md)

Electron 主进程开发模式：

- [目录结构](./backend/directory-structure.md)
- [API 模块](./backend/api-module.md)
- [API 模式](./backend/api-patterns.md)
- [数据库](./backend/database.md)
- [日志](./backend/logging.md)
- [错误处理](./backend/error-handling.md)
- [分页](./backend/pagination.md)
- [环境](./backend/environment.md)
- [类型安全](./backend/type-safety.md)
- [macOS 权限](./backend/macos-permissions.md)
- [文本输入](./backend/text-input.md)

### [共享](./shared/index.md)

横切关注点：

- [TypeScript 约定](./shared/typescript.md)
- [代码质量](./shared/code-quality.md)
- [Git 约定](./shared/git-conventions.md)
- [时间戳处理](./shared/timestamp.md)
- [pnpm + Electron 配置](./shared/pnpm-electron-setup.md)

### [指南](./guides/index.md)

开发思考指南：

- [实现前检查清单](./guides/pre-implementation-checklist.md)
- [跨层思考指南](./guides/cross-layer-thinking-guide.md)
- [代码复用思考指南](./guides/code-reuse-thinking-guide.md)
- [Bug 根因思考指南](./guides/bug-root-cause-thinking-guide.md)
- [数据库 Schema 变更指南](./guides/db-schema-change-guide.md)
- [事务一致性指南](./guides/transaction-consistency-guide.md)
- [语义变更检查清单](./guides/semantic-change-checklist.md)

### [重大问题 / 陷阱](./big-question/index.md)

常见问题和解决方案：

- [原生模块打包](./big-question/native-module-packaging.md)
- [原生模块复杂依赖](./big-question/native-module-complex-deps.md)
- [IPC Handler 注册](./big-question/ipc-handler-registration.md)
- [网络栈差异](./big-question/network-stack-differences.md)
- [事务静默失败](./big-question/transaction-silent-failure.md)
- [React useState 函数](./big-question/react-usestate-function.md)
- [CSS Flex 居中](./big-question/css-flex-centering.md)
- [时间戳精度](./big-question/timestamp-precision.md)
- [Bluetooth HID 设备](./big-question/bluetooth-hid-device.md)
- [全局键盘钩子](./big-question/global-keyboard-hooks.md)

## 技术栈

- **前端**：React 18、TypeScript、TanStack Query、Tailwind CSS
- **后端**：Electron（主进程）、better-sqlite3、TypeScript
- **IPC**：类型安全的 contextBridge 模式
- **构建**：Vite、electron-builder

## 用法

这些指南可用作：

1. **新项目模板** - 为新的 Electron 项目复制整套结构
2. **参考文档** - 实现功能时查阅具体指南
3. **代码审查清单** - 根据既定模式验证实现
4. **入职材料** - 帮助新开发者理解项目约定
