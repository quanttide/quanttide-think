---
name: devops-code-init
description: 初始化新的代码仓库，包括创建 GitHub 仓库、选择许可证、添加 README、首次提交推送。适用于需要从零搭建新仓库的场景，支持公共/私有仓库，支持子模块集成。
---

# DevOps Code Init — 代码仓库初始化

当用户要求初始化一个新的代码仓库时，按以下步骤执行。

## 1. 确认仓库信息

向用户确认或直接使用用户已指定的：

- **仓库名**（GitHub 上 `{owner}/{repo}` 格式）
- **仓库类型**：文档型还是代码型，决定许可证
- **可见性**：默认 **public**
- **描述**：简短说明（可选）
- **子模块路径**（可选）：如果新仓库要作为父仓库的子模块，确认在父项目中的路径
- **初始文件**：一般至少需创建 `README.md`

### 仓库命名规范

若仓库属于 **量潮认知工程** 项目体系，仓库名遵循以下模式：

| 角色 | 仓库名模式 | 示例 |
|------|-----------|------|
| 应用层 | `qt{app}-think` | `qtcloud-think` |
| 陈述性记忆（data/） | `quanttide-{name}-of-cognitive-engineering` | `quanttide-context-of-cognitive-engineering` |
| 程序性记忆（docs/） | `quanttide-{name}-of-cognitive-engineering` | `quanttide-essay-of-cognitive-engineering` |
| 实验室（examples/） | `quanttide-laboratory-of-cognitive-engineering` | 固定名称 |

组织（owner）统一为 `quanttide`。

## 2. 创建 GitHub 仓库

```bash
gh repo create {owner}/{repo} --{visibility} --description "{描述}"
```

如 `gh` 不可用，告知用户手动创建并返回继续。

## 3. 添加许可证文件

根据仓库类型选择许可证：

| 仓库类型 | 许可证 | 文件名 | 说明 |
|---------|--------|--------|------|
| **文档型**（文档、知识库、规范、画廊等） | **CC BY 4.0** | `LICENSE` | 知识共享-署名 4.0 国际 |
| **代码型**（应用程序、库、工具等） | **Apache 2.0** | `LICENSE` | Apache License 2.0 |
| **混合型**（同时包含文档和代码） | 两者兼用 | `LICENSE` + `LICENSE-APACHE` | CC BY 4.0 覆盖文档，Apache 2.0 覆盖代码 |

可通过 `https://api.github.com/licenses/{license-key}` 获取许可证模板正文（`body` 字段），支持的 key：

| 许可证 | key |
|--------|-----|
| CC BY 4.0 | `cc-by-4.0` |
| Apache 2.0 | `apache-2.0` |
| MIT | `mit` |
| GPL 3.0 | `gpl-3.0` |

将获取到的 `body` 写入仓库根目录的许可证文件。

## 4. 添加 README

在仓库根目录创建 `README.md`，格式：

```markdown
# {repo-name}
{描述}
```

## 5. 在子模块工作树中初始化 Git

如果仓库作为子模块添加，工作树中已有 `.git` 文件指向父仓库的 submodule git dir，直接在此工作树下执行：

```bash
# 定位子模块的 git dir
GIT_DIR=$(cat {submodule-path}/.git | sed 's/gitdir: //')
git --git-dir="$GIT_DIR" --work-tree={submodule-path} add LICENSE README.md
git --git-dir="$GIT_DIR" --work-tree={submodule-path} commit -m "chore: init"
git --git-dir="$GIT_DIR" --work-tree={submodule-path} branch -M main
git --git-dir="$GIT_DIR" --work-tree={submodule-path} remote add origin https://github.com/{owner}/{repo}.git
git --git-dir="$GIT_DIR" --work-tree={submodule-path} push -u origin main
```

如果不是子模块，直接在本地仓库目录中执行标准 Git 流程：

```bash
git init
git add .
git commit -m "chore: init"
git branch -M main
git remote add origin https://github.com/{owner}/{repo}.git
git push -u origin main
```

## 6. 更新父仓库（如有）

若新仓库是现有项目的子模块：

1. 确认子模块已正确注册（`.gitmodules`、`AGENTS.md` 已有条目）并已暂存。
2. `git add {submodule-path} .gitmodules AGENTS.md`
3. `git commit -m "chore: add {submodule-path} submodule"`
4. `git push`

## 7. 量潮认知工程规范

当仓库属于 **量潮认知工程** 项目体系时，遵循以下约定：

- **默认公开**，除非用户明确要求私有
- **描述**应符合仓库的认知角色。参考 `quanttide-think/AGENTS.md` 中的子模块列表：
  - 陈述性记忆（data/）："情境"、"日志"、"画像"、"归档"、"洞察"
  - 程序性记忆（docs/）："规范"、"手册"、"画廊"、"工作札记"
  - 应用层：如"思考云"
  - 实验室：如"示例与实验资产"
- **子模块路径与仓库名**严格遵循 `quanttide-think/AGENTS.md` 的约定
- **文档型仓库**使用 CC BY 4.0，**应用/代码型仓库**使用 Apache 2.0
