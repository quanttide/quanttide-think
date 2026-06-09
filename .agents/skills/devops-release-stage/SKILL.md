---
name: devops-release-stage
description: 使用 qtcloud-devops CLI 将版本标记为 Staged（预发布）状态。适用于发布流程中的版本冻结与标记环节。
---

# DevOps Release Stage — 标记版本

## 流程

1. **CHANGELOG** — 在 `packages/rust/CHANGELOG.md` 添加版本条目。
2. **Cargo.toml** — 更新 `packages/rust/Cargo.toml` 的 `version` 字段。
3. **构建验证** — `cd packages/rust && cargo build && cargo test`。
4. **提交推送** — `git add -A && git commit -m "chore: bump to <VERSION>" && git push`。
5. **Stage** — 在 `packages/rust/` 目录下执行 `stage` 命令。

## 用法

```bash
qtcloud-devops release stage --version <VERSION>
```

`<VERSION>` 传入完整 tag 名，例如 `rust/v0.1.0-alpha.9`。`stage` 会用该字符串直接创建 tag。

## 示例

```bash
qtcloud-devops release stage --version rust/v0.1.0-alpha.9
```

## 注意

- 版本号不可逆，crates.io 不允许多次发布同一版本。
- CHANGELOG 必须在 `stage` 前更新，否则命令报错退出。
- tag 格式必须匹配 CI 工作流的 tag 前缀规则（当前 `refs/tags/rust/`）。
