---
name: devops-release-stage
description: 使用 qtcloud-devops CLI 将版本标记为 Staged（预发布）状态。适用于发布流程中的版本冻结与标记环节。
---

# DevOps Release Stage — 标记版本

## 流程

1. **CHANGELOG** — 在 `packages/rust/CHANGELOG.md` 和根目录 `CHANGELOG.md` 添加版本条目。
2. **Cargo.toml** — 更新 `packages/rust/Cargo.toml` 的 `version` 字段。
3. **构建验证** — `cd packages/rust && cargo build && cargo test`。
4. **提交推送** — `git add -A && git commit -m "chore: bump to <VERSION>" && git push`。
5. **Stage** — 执行 `stage` 命令创建 tag 和 GitHub Release。

## 用法

```bash
qtcloud-devops release stage --version <VERSION>
```

`<VERSION>` 使用语义化版本号，如 `v0.1.0-alpha.8`、`v0.1.0`。

## 示例

```bash
qtcloud-devops release stage --version v0.1.0-alpha.8
```

## 注意事项

- **版本号不可逆**：crates.io 不允许多次发布同一版本。发错了只能发新版本，不要删 tag 重发。
- **CI 触发**：`stage` 创建 GitHub Release 后，`rust-publish` 工作流自动发布到 crates.io。如果 CI 未触发，检查 workflow 的 tag 匹配规则。
- **spec 先行**：新增字段或类型时，先定稿 spec 文档（`docs/specification/`），再实现 toolkit。
- **CHANGELOG**：必须在执行 `stage` 前更新，否则命令会报错退出。
