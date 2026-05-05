# CLAUDE.md

本项目默认遵守 `AGENTS.md` 中定义的 `OpenSpec + Superpowers` 交付工作流。`AGENTS.md` 是完整 source of truth；本文件只保留 Claude Code 入口提醒，避免重复维护两套规则。

## Priority

1. 用户的直接请求
2. `AGENTS.md`
3. 本仓库的 `SKILL.md`，或安装到目标项目后的 `.agents/skills/openspec-delivery/SKILL.md`
4. Claude Code 默认行为

## Project Skill

本仓库是 `openspec-delivery` skill 源仓库，workflow skill 位于：

`SKILL.md`

安装到目标项目后，通常位于：

`.agents/skills/openspec-delivery/SKILL.md`

## Language

当用户使用中文沟通时，设计讨论、计划、review、verification 和最终说明默认使用中文。

代码、命令、文件路径、identifier，以及 `proposal.md`、`design.md`、`tasks.md`、`spec.md` 等标准 artifact 文件名保持原文。

创建或更新 OpenSpec artifacts 后，必须检查 `proposal.md`、`design.md`、`tasks.md` 和 `specs/**/spec.md` 的正文语言。中文会话中正文以中文为主，只保留 OpenSpec 结构标记和必要技术英文。

## Behavioral Reminders

实现前先明确假设、歧义和取舍；不能安全假设时先问。

先判断任务规模，按 `SKILL.md` 的 `Token 预算与轻量路径` 选择 tiny、small、meaningful 或 substantial 路径。小改动不要默认展开完整 OpenSpec、Superpowers、references 或长输出。

读取上下文时先定位再读取：优先 `rg`、文件列表和 targeted snippets，只加载当前任务需要的片段。

优先选择满足需求的最小方案，不添加未请求的抽象、配置化或未来扩展点。

只做外科手术式改动。每个 changed line 都应能追溯到用户请求、OpenSpec artifacts 或 verification 需要；无关清理只报告，不顺手修改。

将任务转成可验证目标。多步骤工作要给每个关键步骤定义 verification point，完成前必须有新鲜的验证证据。

使用 subagent、外部模型或工具时，Claude 仍负责最终整合、复核和验证；不要把工具输出直接当作事实或完成状态。

Superpowers 也按需启用：只有当前阶段马上需要对应 skill 时才读它，别一次性展开完整执行栈。

最终回复默认简短，只说明核心文件、关键变化、verification 结果和剩余风险。

如果 `code-review-graph` MCP 工具可用，先用它做 substantial review 或跨文件依赖分析，再做人工作业；只带回高风险节点，不复述整张图。

## Frontend Rule

frontend work 在写 code 前必须先使用 `web-design-engineer`。视觉方向不清晰时先用 `ui-ux-pro-max`，已有明确设计系统或落地方案时优先用 `frontend-design`。

## Maintenance

不要在本文件复制完整 workflow。长期维护以 `AGENTS.md`、本仓库 `SKILL.md` 和安装后的 `.agents/skills/openspec-delivery/SKILL.md` 为准。
