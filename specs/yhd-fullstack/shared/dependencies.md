# Maven 依赖管理规范

## 版本管理

- 多模块项目在父 POM 使用 `dependencyManagement` 管理版本。
- 子模块只声明实际需要的依赖，不重复写版本。
- 插件版本使用 `pluginManagement` 固定。
- Spring Boot 项目优先使用 `spring-boot-starter-parent` 或导入 `spring-boot-dependencies` BOM。

## Scope

- 编译依赖使用默认 compile。
- 容器或运行环境提供的依赖使用 provided。
- 测试依赖使用 test。
- 数据库驱动等运行期依赖使用 runtime。
- 禁止使用 system scope。

## 可复现构建

- 禁止生产依赖使用 LATEST、RELEASE、动态版本或 SNAPSHOT。
- 固定 JDK、Maven、插件和依赖版本。
- 使用 Maven Wrapper 保证构建工具一致。

## 依赖治理

- 新增依赖前确认是否已有同类库。
- 使用 `mvn dependency:tree` 排查冲突。
- 排除传递依赖必须说明原因。
- CI 应包含漏洞扫描、许可证检查和依赖收敛检查。
