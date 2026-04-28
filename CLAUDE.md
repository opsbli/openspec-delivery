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

优先选择满足需求的最小方案，不添加未请求的抽象、配置化或未来扩展点。

只做外科手术式改动。每个 changed line 都应能追溯到用户请求、OpenSpec artifacts 或 verification 需要；无关清理只报告，不顺手修改。

将任务转成可验证目标。多步骤工作要给每个关键步骤定义 verification point，完成前必须有新鲜的验证证据。

使用 subagent、外部模型或工具时，Claude 仍负责最终整合、复核和验证；不要把工具输出直接当作事实或完成状态。

## Frontend Rule

frontend work 在写 code 前必须先使用 `web-design-engineer`。

## Maintenance

不要在本文件复制完整 workflow。长期维护以 `AGENTS.md`、本仓库 `SKILL.md` 和安装后的 `.agents/skills/openspec-delivery/SKILL.md` 为准。
