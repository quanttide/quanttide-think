---
name: devops-code-sync
description: 同步子模块（组件）变更到远端，包括提交、推送和父仓库指针更新。适用于修改了子模块内容后需要同步到 GitHub 的场景。
---

# DevOps Code Sync — 组件同步

当用户要求同步子模块（组件）变更到远端时，按以下步骤执行。

**依赖：** `qtcloud-devops >= 0.5.0`

## 1. 查看组件状态

在父仓库根目录执行，确认哪些组件存在待同步的变更：

```bash
qtcloud-devops code status
```

输出示例：

```
仓库: /path/to/parent-repo
组件总数: 16
待处理: 2
  packages/quanttide-agent-toolkit  待提交 (本地有变更)
  examples/default                  待推送 (领先 1 提交)
```

状态含义：
- **待提交** — 子模块工作区有未提交的本地变更
- **待推送** — 子模块有已提交但未推送到远端的 commit

## 2. 同步组件

### 同步全部组件

```bash
qtcloud-devops code sync
```

### 同步指定组件

```bash
qtcloud-devops code sync {component-name}
```

`{component-name}` 使用 `code status` 输出中显示的组件名称。

### 试运行（不实际执行）

```bash
qtcloud-devops code sync --dry-run
```

该命令自动执行：
1. 在子模块内 `git add → git commit → git push`
2. 回到父仓库更新子模块指针（`git add → git commit → git push`）

### 手动流程（qtcloud-devops 不可用时）

```bash
# 1. 进入子模块，提交并推送
cd {submodule-path}
git add .
git commit -m "chore: {描述变更内容}"
git push

# 2. 回到父仓库，更新子模块指针
cd {parent-path}
git add {submodule-path}
git commit -m "chore: update {submodule-path}"
git push
```

## 3. 确认同步结果

```bash
qtcloud-devops code status
```

确认待处理数为 0。

## 4. 量潮认知工程约定

- 修改子模块后，**必须先推送子模块本身，再推送父仓库**的子模块指针变更。`code sync` 自动保证顺序。
- 子模块的 `AGENTS.md` 记录了该组件的认知角色，更新内容时应与角色保持一致。
