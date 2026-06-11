# AGENTS.md - 量潮认知工程领域仓库

## 核心工作流

### git 操作规范

1. **操作前**：先 `git fetch` 和 `git pull`，确保本地与远程同步。
2. **操作后**：每次提交必须同时推送到远程（`git push`），避免本地提交丢失。
3. **子模块**：修改子模块内容后，先提交推送子模块本身，再回到主仓库提交推送子模块指针变更。

### 子模块列表

| 路径 | 仓库 | 认知角色 |
|------|------|---------|
| `apps/qtcloud-think` | `quanttide/qtcloud-think` | 应用层（思考云） |
| `packages/quanttide-agent-toolkit` | `quanttide/quanttide-agent-toolkit` | 应用层（智能体工具箱） |
| `packages/quanttide-think-toolkit` | `quanttide/quanttide-think-toolkit` | 应用层（工具箱） |
| `data/archive` | `quanttide/quanttide-archive-of-cognitive-engineering` | 陈述性记忆 · **终点** |
| `data/context` | `quanttide/quanttide-context-of-cognitive-engineering` | 陈述性记忆 · **起点** |
| `data/library` | `quanttide/quanttide-library-of-cognitive-engineering` | 陈述性记忆 |
| `data/insight` | `quanttide/quanttide-insight-of-cognitive-engineering` | 陈述性记忆 |
| `data/intention` | `quanttide/quanttide-intention-of-cognitive-engineering` | 陈述性记忆 |
| `data/journal` | `quanttide/quanttide-journal-of-cognitive-engineering` | 陈述性记忆 |
| `data/profile` | `quanttide/quanttide-profile-of-cognitive-engineering` | 陈述性记忆 |
| `data/roadmap` | `quanttide/quanttide-roadmap-of-cognitive-engineering` | 陈述性记忆 |
| `data/report` | `quanttide/quanttide-report-of-cognitive-engineering` | 陈述性记忆 |
| `docs/essay` | `quanttide/quanttide-essay-of-cognitive-engineering` | 程序性记忆 |
| `docs/gallery` | `quanttide/quanttide-gallery-of-cognitive-engineering` | 程序性记忆 |
| `docs/handbook` | `quanttide/quanttide-handbook-of-cognitive-engineering` | 程序性记忆 |
| `docs/specification` | `quanttide/quanttide-specification-of-cognitive-engineering` | 程序性记忆 |
| `examples/default` | `quanttide/quanttide-laboratory-of-cognitive-engineering` | 实验室（示例与实验资产） |

### 认知模型

```
── 生命周期 ──

  context（起点）← 九宫格分类 → archive（终点）

── 记忆类型 ──

  data/     陈述性记忆（事实性知识：情境、日志、画像、归档）
  docs/     程序性记忆（操作性知识：规范、手册、画廊）
```

- `data/` 存放事实性知识 —— 发生了什么、当前状态是什么。其中 **context 是知识生命周期的起点**（工作语境），**archive 是终点**（历史归档）。
- `docs/` 存放操作性知识 —— 怎么做、标准是什么、成果展示。
- 起点（context）与终点（archive）之间，所有子模块构成一个九宫格分类体系。

## 数据流

```
外部认知工程生态（GitHub / 论文 / 书籍）
    ↓ 收集
data/library ← 图书馆（外部知识索引）

原始日志（examples/default/data/）
    ↓ 提炼
docs/gallery ← 事实源
    ↓ 引用
examples/default/ ← 实验室（实验与产出）
```

- **图书馆（library）** 是外部输入，收集理论框架和开源产品，回答「别人做了什么」
- **实验室（lab）** 是内部输出，用 gallery 事实源和内部数据模型进行认知工程实验，回答「我们自己能做出什么」
- 两者不直接耦合，但互补：图书馆提供理论参考和产品对标，实验室验证哪些理论可以工程化
- `docs/gallery` 是唯一的**事实源**，lab（`examples/default/`）是工具消费方
- lab 中的 `project-11` 直接读取 `docs/gallery/` 的 YAML 文件
- lab 生成的推理产物（`reports/`、`schemas.yaml`）不写回 gallery
- gallery 的数据只能通过人工审核后修改，不通过工具自动写入
- 修改子模块内容时，**先提交推送子模块本身，再回到主仓库提交推送子模块指针变更**
