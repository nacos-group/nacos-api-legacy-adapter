# nacos-api-legacy-adapter

[English](README.md) | 简体中文

独立仓库：<https://github.com/nacos-group/nacos-api-legacy-adapter>

本模块为 **兼容旧版本 HTTP API** 的适配层，提供 legacy v1/v2 接口的兼容实现，便于在 Nacos 升级后仍能支持尚未改造的旧客户端或调用方。

**使用说明：**

- 本模块**仅面向**迫切需要使用旧版本 API、或正在对旧客户端进行改造的用户**临时使用**。
- **不保证**未来 Nacos 版本仍会保留或继续支持本模块，建议尽快迁移到新版本 API 与客户端。

---

## 构建

- **环境**：JDK 17+，Maven 3.6+
- **版本对齐**：`pom.xml` 中 `nacos.version` 需与目标 Nacos 服务端版本一致（如 3.2.0）。
- 若使用 Nacos 快照，需在 Maven 中配置 Nacos 快照仓库。

```bash
mvn clean install
```

构建产物：`target/nacos-api-legacy-adapter-${version}.jar`。

---

## 可插拔使用方式

本模块**非默认依赖**，仅当 JAR 出现在运行期 classpath 时生效；默认 Nacos 构建与启动不包含本模块。

### 需要 legacy API 时如何引入

- **使用发行包**：将 `nacos-api-legacy-adapter-${version}.jar` 放入 Nacos 可加载的目录（如 `plugins`），保证在 classpath 中即可。
- **自定义/嵌入应用**：在构建或运行 classpath 中增加对 `nacos-api-legacy-adapter` 的 Maven 依赖。

无需改代码或配置；classpath 中存在该 JAR 时，模块会自动加载并生效。

---

## 开发者须知

由于本模块不保证未来版本 Nacos 仍然支持本模块，若开发者希望自行基于本模块自行适配或协助社区更新维护此模块，应该先拉取 [alibaba/nacos](https://github.com/alibaba/nacos) 仓库，并按照文档 [https://nacos.io/docs/latest/contribution/source-code-run-and-start/](https://nacos.io/docs/latest/contribution/source-code-run-and-start/) 进行 Nacos 主服务的本地编译和打包，因为本模块依赖的 Nacos 服务不会发布到中央仓库中。
