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

## 注意

- 版本号不可逆，crates.io 不允许多次发布同一版本。
- CHANGELOG 必须在 `stage` 前更新，否则命令报错退出。
