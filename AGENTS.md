# AGENTS.md - 量潮认知工程领域仓库

## 核心工作流

### git 操作规范

1. **操作前**：先 `git fetch` 和 `git pull`，确保本地与远程同步。
2. **操作后**：每次提交必须同时推送到远程（`git push`），避免本地提交丢失。
3. **子模块**：修改子模块内容后，先提交推送子模块本身，再回到主仓库提交推送子模块指针变更。

### 子模块列表

| 路径 | 仓库 |
|------|------|
| `apps/qtcloud-think` | `quanttide/qtcloud-think` |
| `data/archive` | `quanttide/quanttide-archive-of-cognitive-engineering` |
| `data/profile` | `quanttide/quanttide-profile-of-cognitive-engineering` |
| `docs/gallery` | `quanttide/quanttide-gallery-of-cognitive-engineering` |
| `docs/handbook` | `quanttide/quanttide-handbook-of-cognitive-engineering` |
| `docs/journal` | `quanttide/quanttide-journal-of-cognitive-engineering` |
| `docs/context` | `quanttide/quanttide-context-of-cognitive-engineering` |
| `docs/specification` | `quanttide/quanttide-specification-of-cognitive-engineering` |
| `examples/default` | `quanttide/quanttide-laboratory-of-cognitive-engineering` |
