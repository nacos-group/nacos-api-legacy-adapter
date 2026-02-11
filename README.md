# nacos-api-legacy-adapter

English | [简体中文](README_CN.md)

Standalone Repository: <https://github.com/nacos-group/nacos-api-legacy-adapter>

This module is a **compatibility layer for legacy HTTP APIs**, providing compatible implementations of legacy v1/v2 interfaces to support old clients or callers that have not yet been migrated after Nacos upgrades.

**Usage Instructions:**

- This module is **only intended for temporary use** by users who urgently need legacy APIs or are in the process of migrating old clients.
- There is **no guarantee** that future Nacos versions will retain or continue to support this module. It is recommended to migrate to the new version APIs and clients as soon as possible.

---

## Build

- **Environment**: JDK 17+, Maven 3.6+
- **Version Alignment**: The `nacos.version` in `pom.xml` must match the target Nacos server version (e.g., 3.2.0).
- If using Nacos snapshots, configure the Nacos snapshot repository in Maven.

```bash
mvn clean install
```

Build artifact: `target/nacos-api-legacy-adapter-${version}.jar`.

---

## Pluggable Usage

This module is **not a default dependency** and only takes effect when the JAR is present in the runtime classpath. The default Nacos build and startup do not include this module.

### How to Include Legacy API Support

- **Using Release Package**: Place `nacos-api-legacy-adapter-${version}.jar` in a directory that Nacos can load (such as `plugins`), ensuring it's in the classpath.
- **Custom/Embedded Application**: Add the Maven dependency for `nacos-api-legacy-adapter` in your build or runtime classpath.

No code or configuration changes are required; the module will automatically load and take effect when the JAR is present in the classpath.

---

## Developer Notes

This module is not guaranteed to be supported in future versions of Nacos. If developers wish to adapt this module themselves or help the community maintain and update it, they should follow these steps:

1. First clone the [alibaba/nacos](https://github.com/alibaba/nacos) repository
2. Follow the documentation at [https://nacos.io/docs/latest/contribution/source-code-run-and-start/](https://nacos.io/docs/latest/contribution/source-code-run-and-start/) to compile and package the main Nacos service locally

This is necessary because the Nacos services this module depends on are not published to the central repository.
