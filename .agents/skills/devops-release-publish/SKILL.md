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

## 工作流程

### 1. 查看当前发布状态

在开始发布前，先检查当前是否有进行中的发布记录：

```bash
qtcloud-devops release status
```

输出示例：`当前无发布记录` 表示可以开始新的发布。

### 2. Stage 版本

将版本标记为 Staged 状态：

```bash
qtcloud-devops release stage --version <VERSION>
```

`<VERSION>` 使用语义化版本号，如 `0.1.0`、`1.2.3`。

### 3. Publish 正式发布

将 Staged 版本正式发布上线：

```bash
qtcloud-devops release publish --version <VERSION> [OPTIONS]
```

**选项：**

| 选项 | 说明 |
|------|------|
| `-v, --version <VERSION>` | **必填**。要发布的版本号，必须与 stage 时一致 |
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

### 4. Retire 退役版本（可选）

如需撤销已发布的版本，可将其标记为退役：

```bash
qtcloud-devops release retire --version <VERSION>
```

## 完整发布流程示例

```bash
# 1. 查看状态
qtcloud-devops release status

# 2. Stage
qtcloud-devops release stage --version 0.1.0

# 3. Publish
qtcloud-devops release publish --version 0.1.0 --registry crates -y
```

## 注意事项

- **Stage 必须先于 Publish**：必须先将版本标记为 Staged，才能执行 Publish
- **版本号一致性**：stage、publish、retire 中的 `--version` 必须使用相同的版本号
- **多次发布**：需要发布到多个 registry 时，分别执行多次 `publish` 命令
- **确认提示**：默认有确认提示，生产环境建议先不加 `-y` 确认信息无误，确认后再加 `-y` 执行
