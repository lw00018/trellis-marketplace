# Git 约定

## 提交信息

推荐格式：

```text
type(scope): summary
```

常见 type：

- `feat`：新功能
- `fix`：缺陷修复
- `refactor`：重构
- `test`：测试
- `docs`：文档
- `chore`：构建、依赖、工具调整

## 分支命名

| 分支 | 用途 |
| --- | --- |
| `dev` | 开发分支 |
| `master` | 主分支 |
| `feature/{ticket}-{summary}` | 开发新功能分支 |
| `release/{version}` | 预发布分支 |
| `hotfix/{ticket}-{summary}` | 修补 Bug 分支 |

- 分支名称统一使用小写英文，主分支固定为 `master`，不得使用 `Master`。
- `feature`、`release`、`hotfix` 分支必须从对应基础分支创建，合并后及时删除。

## 合并前检查

- 提交粒度聚焦单一意图。
- 不提交密钥、日志文件、临时文件和 IDE 私有配置。
- 数据库迁移、API 文档和测试与代码一起提交。
- 破坏性变更必须在 PR 描述中明确说明。
