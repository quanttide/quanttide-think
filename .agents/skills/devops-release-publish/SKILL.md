---
name: devops-release-publish
description: 使用 qtcloud-devops CLI 执行正式发布流程，包括 stage、publish、retire、status 等发布生命周期命令。适用于将子模块或应用的新版本标记、发布和退役。
---

# DevOps Release Publish — 发布管理

当用户要求发布新版本时，使用 `qtcloud-devops release` 命令集完成发布生命周期管理。

## 命令概览

```bash
qtcloud-devops release <COMMAND>
```

| 命令 | 作用 |
|------|------|
| `stage` | 将版本标记为 Staged（预发布）状态 |
| `publish` | 将 Staged 版本正式发布到指定仓库源 |
| `retire` | 将已发布版本标记为退役 |
| `status` | 查看当前发布状态 |

## 命令详解

### release status — 查看发布状态

检查当前是否有进行中的发布记录：

```bash
qtcloud-devops release status
```

输出示例：`当前无发布记录` 表示当前无活跃发布。

### release stage — 标记版本

将版本标记为 Staged 状态（语义化版本号，如 `0.1.0`、`1.2.3`）：

```bash
qtcloud-devops release stage --version <VERSION>
```

### release publish — 正式发布

将版本正式发布上线（stage 不是前置条件，可独立使用）：

```bash
qtcloud-devops release publish --version <VERSION> [OPTIONS]
```

**选项：**

| 选项 | 说明 |
|------|------|
| `-v, --version <VERSION>` | **必填**。要发布的版本号 |
| `-y, --yes` | 跳过确认提示，直接执行 |
| `--registry <REGISTRY>` | 指定发布的仓库源。可选值：`py-pi`、`pub-dev`、`crates` |

**示例：**

```bash
# 发布到 crates.io
qtcloud-devops release publish --version 0.1.0 --registry crates

# 快速发布，跳过确认
qtcloud-devops release publish --version 1.0.0 -y

# 发布到多个源需多次执行
qtcloud-devops release publish --version 0.1.0 --registry py-pi
qtcloud-devops release publish --version 0.1.0 --registry pub-dev
```

### release retire — 退役版本

将已发布的版本标记为退役：

```bash
qtcloud-devops release retire --version <VERSION>
```

## 使用示例

```bash
# 直接发布（不 stage）
qtcloud-devops release publish --version 0.1.0 --registry crates -y

# 先 stage 再发布
qtcloud-devops release stage --version 0.1.0
qtcloud-devops release publish --version 0.1.0 --registry crates
```

## 注意事项

- **版本号一致性**：同一版本号在 stage、publish、retire 之间保持语义一致
- **多次发布**：需要发布到多个 registry 时，分别执行多次 `publish` 命令
- **确认提示**：默认有确认提示，生产环境建议先不加 `-y` 确认信息无误，确认后再加 `-y` 执行
