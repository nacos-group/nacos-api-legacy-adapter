# 迁移到独立仓库说明 (nacos-group/nacos-api-legacy-adapter)

本文说明将 `nacos-api-legacy-adapter` 从 nacos 主仓库迁移到 [nacos-group/nacos-api-legacy-adapter](https://github.com/nacos-group/nacos-api-legacy-adapter) 的修改说明。**POM 与主仓库引用已按此完成修改。**

## 结论概览

| 类型 | 是否需要修改 | 说明 |
|------|--------------|------|
| **Java 源码** | **否** | 包名、import、SPI、Spring 配置均保持不变，无需改代码 |
| **pom.xml** | **是** | 必须改为独立根项目 POM，并显式指定 nacos 依赖版本 |
| **README / 文档** | 建议 | 可补充“从独立仓库构建”“版本与 Nacos 对齐”等说明 |

---

## 1. 为何 Java 代码不用改？

- 本模块是 **Nacos 的可插拔插件**：通过依赖 `nacos-core`、`nacos-console` 使用其内部 API（如 `Compatibility`、`ExtractorManager`、`NamespaceOperationService` 等）。
- 迁移到独立仓库后，这些依赖改为从 **Maven 中央仓库**（或指定仓库）拉取，**groupId/artifactId/包名不变**，因此源码和资源文件都无需改动。
- SPI 文件 `META-INF/services/com.alibaba.nacos.sys.filter.NacosPackageExcludeFilter` 和  
  `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports` 也保持不变。

---

## 2. 必须修改：pom.xml

在独立仓库中不能再使用 nacos 主工程的 parent POM，需要：

1. **去掉 `<parent>`**  
   不再引用 `nacos-all`。

2. **补全 GAV**  
   - 保留 `groupId`: `com.alibaba.nacos`  
   - 保留 `artifactId`: `nacos-api-legacy-adapter`  
   - 显式写 `version`（如 `3.2.0-SNAPSHOT` 或与 Nacos 对齐的版本）。

3. **显式指定 nacos 依赖版本**  
   - `nacos-core`、`nacos-console`、`default-auth-plugin` 改为带版本的依赖。  
   - 建议用属性（如 `nacos.version`）统一管理，便于与 Nacos 主版本对齐。

4. **补全构建配置**  
   - 若不再继承父 POM，需自行配置 `maven-compiler-plugin`（如 Java 17）、编码等。

5. **可选**  
   - 将 `scm`、`url` 指向新仓库：  
     `https://github.com/nacos-group/nacos-api-legacy-adapter`  
   - 保留或补充 `licenses` 等元数据。

已在当前目录提供 **独立仓库用的 POM 示例**：`pom-standalone.xml`。  
迁移时可将该文件内容替换现有的 `pom.xml`，再按需调整 `nacos.version` 和 `version`。

---

## 3. 版本对齐建议

- **nacos-api-legacy-adapter** 的版本建议与 **Nacos 服务端** 对齐（例如 Nacos 3.2.x 对应 adapter 3.2.x）。  
- 在独立仓库的 POM 中用 `nacos.version` 指向要兼容的 Nacos 版本；发布 adapter 时保持与 Nacos 主版本一致，可减少用户使用时的版本混淆。

---

## 4. 主仓库 (nacos) 迁移后建议

- 从根 `pom.xml` 的 `<modules>` 中 **移除** `api-legacy-adapter`。
- 若主项目仍需要打包/引用该 adapter：
  - 在 **dependencyManagement** 中改为依赖已发布的坐标，例如：  
    `com.alibaba.nacos:nacos-api-legacy-adapter:${nacos-api-legacy-adapter.version}`  
  - 并在 bootstrap/打包逻辑中通过“从 classpath 或 plugins 目录加载”的方式使用，保持当前可插拔行为。

---

## 5. 小结

- **需要改的**：主要是 **pom.xml**（独立根 POM + 显式 nacos 依赖版本 + 构建配置）。  
- **不需要改的**：Java 代码、包结构、SPI 与 Spring 配置。  
- 使用提供的 `pom-standalone.xml` 作为新仓库的 `pom.xml` 起点即可完成迁移。
