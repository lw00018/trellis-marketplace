# CORS、CSRF 与 Cookie

## 现象

- 本地能登录，生产跨域后 Cookie 不生效。
- OPTIONS 预检失败。
- 开启 Cookie 后出现 CSRF 风险。

## 根因

CORS、Cookie SameSite、Secure、Domain、CSRF Token 和前后端部署域名必须整体设计，不能分别调试。

## 解决方案

- 明确前端域名、API 域名和 Cookie 域。
- 使用 Cookie 认证时设置合适的 SameSite 和 Secure。
- 跨站 Cookie 必须配合 HTTPS 和凭证请求。
- 对状态变更请求启用 CSRF 防护或使用不受 CSRF 影响的认证方式。

## 预防

- 本地、测试、生产分别记录认证拓扑。
- CORS 允许来源使用白名单，不使用任意通配。
- 认证方案变更必须同时评估 XSS 和 CSRF。
