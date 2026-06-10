---
name: devops-release-publish
description: 发布版本到包注册表/分发渠道，包括校验 CHANGELOG、打标签、推送到远端、创建 GitHub Release。适用于代码型仓库的版本发布流程。
---

# DevOps Release Publish — 版本发布

当用户要求发布一个版本时，按以下步骤执行。

**依赖：** `qtcloud-devops >= 0.5.0`

## 1. 确认发布信息

向用户确认：

- **版本号**：格式 `vX.Y.Z` 或 `scope/vX.Y.Z`（如 `cli/v0.5.0`、`rust/v0.1.0`）
- **发布目标**（可选）：CI 发布渠道
  - `py-pi` — PyPI
  - `pub-dev` — pub.dev
  - `crates` — crates.io
- **组件名称**（可选）：如果仓库是多语言 monorepo，指定此次发布的组件名

## 2. 检查发布前置条件

确认以下文件已就绪：

| 文件 | 要求 |
|------|------|
| `CHANGELOG.md` | 已包含本次版本的变更记录 |
| `Cargo.toml` / `pyproject.toml` | 版本号与发布的版本号一致 |
| 工作区 | 干净（无未提交变更） |

## 3. 执行发布

使用 `qtcloud-devops release publish` 一键完成发布：

```bash
# 基础用法
qtcloud-devops release publish --version {version}

# 带发布目标（仅打印提示，不执行 CI 发布）
qtcloud-devops release publish --version {version} --registry {target}

# 跳过用户确认（脚本中使用）
qtcloud-devops release publish --version {version} --yes
```

该命令自动执行：
1. 校验 CHANGELOG 是否包含对应版本条目
2. 创建并推送 Git tag
3. 创建 GitHub Release

## 4. 发布后确认

- 确认 GitHub Release 已创建：`gh release view {tag}`
- 如指定了 `--registry`，提示用户前往 CI 确认包发布状态
- 如果是子模块，回到父仓库更新子模块指针：

```bash
qtcloud-devops code sync {component-name}
```

## 5. 量潮认知工程约定

- 预发布版本（alpha / beta / rc）使用 `scope/vX.Y.Z-{pre}.{N}` 格式，如 `rust/v0.1.0-alpha.10`
- 正式版本去掉预发布后缀，如 `rust/v0.1.0`
- 多语言 monorepo 中务必使用 scope 前缀区分，避免 tag 冲突
