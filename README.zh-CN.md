# OpenSpec Delivery Skill

**一个把 OpenSpec 规格流和 Superpowers 执行纪律组合起来的可复用 AI Agent 技能。**

[English](./README.md)

---

## 这是什么？

这是一个面向 AI 编程代理的可复用 **Skill**，把两层能力组合在一起：

- **OpenSpec**：负责规格工件、变更追溯、验证与归档
- **Superpowers**：负责执行纪律，包括 brainstorming、worktree 隔离、任务规划、TDD、审查和完成前验证

它适合那些不想靠“聊天感觉”推进，而是希望 AI 按工程化交付流程开发的软件项目。

### 它解决什么问题？

AI 写代码很快，但如果缺少流程约束，通常会出现这些问题：

- 实现过程中逐渐偏离需求
- 在规格和验收边界不清晰时就开始编码
- 任务太大，无法稳定做 TDD
- 前端实现跳过设计推导，直接堆界面
- 没有验证证据就宣布完成

### 它怎么解决？

这个 Skill 明确分工：

- **OpenSpec 回答**：做什么、为什么做、做到什么程度算完成
- **Superpowers 回答**：怎么稳妥地执行、审查、验证和收尾

并补充了几条关键执行规则：

- 任务优先拆到 2-5 分钟粒度
- 重要任务要求写清 DoD
- 两阶段审查：先规格合规，再代码质量
- 非 trivial 变更优先使用 git worktree 隔离
- 前端任务必须先走 `web-design-engineer`

---

## 快速上手

把 skill 放进项目目录：

```text
your-project/
├── AGENTS.md
├── .agents/
│   └── skills/
│       └── openspec-delivery/
│           ├── SKILL.md
│           ├── README.md
│           ├── README.zh-CN.md
│           └── agents/
│               └── openai.yaml
└── openspec/
```

建议同时具备：

- 项目级 `AGENTS.md`
- `openspec/changes/<change-id>/` 下的变更工件

---

## 它覆盖什么？

| 层级 | 职责 |
|---|---|
| OpenSpec | `proposal.md`、`design.md`、`specs/**/spec.md`、`tasks.md`、`verify`、`archive` |
| Superpowers | brainstorming、worktree、planning、TDD、debugging、review、verification |
| 前端规则 | UI 实现前强制使用 `web-design-engineer` |

典型场景：

- 功能开发
- 有回归风险的 bugfix
- 需要规格与设计联动的前端变更
- 需要显式边界控制的重构
- 任何希望以“可归档变更单元”方式推进的开发任务

---

## 工作方式

### 流程形态

```text
想法 / 需求
  -> OpenSpec explore / propose / ff
  -> proposal + design + specs + tasks
  -> Superpowers 执行
  -> verify
  -> archive
```

### 核心规则

1. 从 OpenSpec 工件出发，不从聊天记忆出发。
2. 把 `tasks.md` 当成执行控制面，而不只是清单。
3. 先把大任务拆小，再开始编码。
4. 功能和 bugfix 默认走 TDD。
5. 规格审查和代码质量审查必须分开。
6. 前端任务先用 `web-design-engineer`。
7. 没有验证证据，不要声称完成。

---

## 推荐的执行技能组合

不要一次性加载所有 Superpowers skills。优先使用小而精的活动集合。

### 核心集合

- `superpowers:test-driven-development`
- `superpowers:verification-before-completion`

### 常用按需集合

- `superpowers:writing-plans`
- `superpowers:requesting-code-review`
- `superpowers:systematic-debugging`

### 可选集合

- `superpowers:brainstorming`
- `superpowers:using-git-worktrees`
- `superpowers:subagent-driven-development`
- `superpowers:finishing-a-development-branch`

---

## 前端规则

当任务涉及 UI、页面、组件、交互、仪表盘或原型时：

- 实现前先用 `web-design-engineer`
- 关键 UX 约束要写回 `design.md` 或 specs
- 不要把前端当成单纯的代码生成任务

这是一条硬规则，不是建议。

---

## 文件说明

- [SKILL.md](./SKILL.md)：主 workflow 指令
- [agents/openai.yaml](./agents/openai.yaml)：技能元数据
- [references/example-change.md](./references/example-change.md)：`proposal/design/spec/tasks` 一体化示例
- [references/recommended-skill-sets.md](./references/recommended-skill-sets.md)：推荐的 Superpowers 加载组合
- [references/failure-recovery.md](./references/failure-recovery.md)：常见 workflow 失败路径修复手册

---

## 许可证

MIT
