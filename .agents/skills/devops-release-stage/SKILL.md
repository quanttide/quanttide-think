---
name: devops-release-stage
description: 使用 qtcloud-devops CLI 将版本标记为 Staged（预发布）状态。适用于发布流程中的版本冻结与标记环节。
---

# DevOps Release Stage — 标记版本

将版本标记为 Staged（预发布）状态。

## 用法

```bash
qtcloud-devops release stage --version <VERSION>
```

`<VERSION>` 使用语义化版本号，如 `0.1.0`、`1.2.3`。

## 示例

```bash
qtcloud-devops release stage --version 0.1.0
qtcloud-devops release stage --version 1.0.0
```
