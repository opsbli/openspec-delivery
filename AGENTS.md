# AGENTS.md

本项目默认使用 `OpenSpec + Superpowers` 交付工作流。

## 优先级

按以下顺序应用约束：

1. 用户的直接请求
2. 本 `AGENTS.md`
3. 本仓库的 `SKILL.md`，或安装到目标项目后的 `.agents/skills/openspec-delivery/SKILL.md`
4. 其他通用默认行为

## 语言

当用户使用中文沟通时，以下内容默认使用中文：

- 设计讨论
- 工作流指导
- 实施计划
- code review 总结
- verification 输出
- 最终交付说明

代码、命令、文件路径、identifier，以及 `proposal.md`、`design.md`、`tasks.md`、`spec.md` 等标准 artifact 文件名保持原文。

### OpenSpec Artifact 语言检查

当用户使用中文沟通时，OpenSpec artifacts 的说明正文必须以中文为主：

- `proposal.md`、`design.md`、`tasks.md` 和 `specs/**/spec.md` 的叙述、需求说明、任务描述和 verification 说明默认使用中文。
- 保留 OpenSpec 解析标记、命令、代码、文件路径、identifier 和必要英文术语，例如 `## ADDED Requirements`、`### Requirement:`、`#### Scenario:`、`SHALL`、`WHEN`、`THEN`、`AND`。
- 创建或更新 artifacts 后，必须重新读取并检查语言；如果正文意外使用英文，先修正 artifacts，再继续实现或总结。

## 会话启动锚定

当用户在新会话中提出任务时，先用中文简短说明：

1. 用户要你做什么。
2. 你计划用什么步骤完成。
3. 当前还缺什么信息，或说明没有明显缺口。

完成这段说明后再执行 tool call、文件操作或实现推理。若用户明确要求直接执行极小命令，可保持一句话锚定。

## 必需工作流

任何 feature、bugfix、refactor 或行为变更都必须遵循：

1. 从 OpenSpec artifacts 开始，不做 ad hoc coding。
2. 工作保持在 `openspec/changes/<change-id>/` 内。
3. implementation 前必须创建或更新 `proposal.md`。
4. 当变更影响 architecture、data flow、API、UX 或跨模块行为时，必须维护 `design.md`。
5. 对 externally observable behavior 维护 `specs/**/spec.md`。
6. 将 `tasks.md` 作为 implementation checklist 和执行 source of truth。
7. 显式维护 non-goals，防止实现阶段随意扩大 scope。
8. 只有当 change scope、tasks 和 acceptance criteria 足够清晰时才进入 implementation。
9. 关闭工作前必须同时验证 code quality checks 和 OpenSpec artifact consistency。

## Superpowers 策略

在 OpenSpec 之上使用 Superpowers process skills 作为执行层：

- 创意 feature design、product shaping 或行为变更前使用 `superpowers:brainstorming`。
- 非平凡隔离变更且 repo 支持时使用 `superpowers:using-git-worktrees`。
- 多步骤 implementation 前使用 `superpowers:writing-plans`。
- 只有在 approved plan 中存在独立任务和明确 ownership 时，才使用 `superpowers:subagent-driven-development`。
- 任何 feature 或 bugfix implementation 前使用 `superpowers:test-driven-development`。
- 修复 bug、flaky tests 或 unexpected behavior 前使用 `superpowers:systematic-debugging`。
- substantial implementation 后使用 `superpowers:requesting-code-review`，分两轮：先 spec compliance，再 code quality。
- 声称完成前使用 `superpowers:verification-before-completion`。
- 只有当任务真正独立且 ownership 明确时，才使用 `superpowers:dispatching-parallel-agents`。
- 当任务包含 merge、PR 或 branch cleanup 时使用 `superpowers:finishing-a-development-branch`。

优先加载小而准确的 active skill set，不要一次性加载所有 skill。

## Coding Agent 行为规则

这些规则适用于所有 coding、文档规则、配置和 workflow 变更；它们补强 OpenSpec 流程，不替代 OpenSpec。

### 先想清楚再实现

- 不隐藏不确定性。发现需求、API、数据契约或目标有多种解释时，先说明歧义。
- 能做低风险假设时，明确写出假设再继续；不能安全假设时，先问用户。
- 如果存在更简单的方案，主动指出，并说明取舍。
- 当用户请求与现有 artifacts、代码事实或工具能力冲突时，先停下来协调，不静默选择一边。

### 简单优先

- 只实现用户请求和 OpenSpec artifacts 要求的行为。
- 不为单次使用代码添加抽象、插件化、配置化或未来扩展点。
- 不添加 speculative error handling；只处理真实输入边界、已知失败模式和 acceptance criteria 覆盖的情况。
- 如果实现明显比问题复杂，先缩小设计或拆分任务，而不是继续堆代码。

### 外科手术式改动

- 每个 changed line 都应能追溯到用户请求、OpenSpec artifacts、测试或 verification 需要。
- 不顺手重构、格式化、改注释或清理相邻代码。
- 匹配现有风格和局部模式，即使个人偏好不同。
- 发现无关 dead code、坏味道或风险时，在总结中说明；除非它阻塞当前任务，否则不直接修改。
- 只清理本次变更造成的 unused imports、变量、函数或 orphaned artifacts。

### 目标驱动执行

- 将宽泛任务转成可验证目标，例如：bugfix 需要复现检查，refactor 需要前后行为验证，文档规则变更需要 artifact consistency 检查。
- 多步骤任务要写出每个关键步骤的 verification point。
- 不能用“看起来可以”替代测试、lint、build、typecheck、OpenSpec verify 或 targeted review。
- 当 verification 失败时，把 failure 映射回具体 task，再修复和复验。

## 任务粒度

implementation 前需要将任务拆成小颗粒度：

- 可行时优先拆成 2-5 分钟任务。
- 每个任务只对应一个 narrow behavior 或一个小范围 code change。
- 容易误解的任务要补充简短 DoD。
- 可行时，每个任务都应暗含一个 failing test 和一个 boundary case。

如果任务过大，无法清晰测试或 review，先拆分再实现。

## Worktree 规则

非平凡变更遵循：

1. 每个 active change 优先对应一个 active worktree。
2. 使用可预测命名，例如 `../<repo>-feature-<change>` 或 `../<repo>-<change>-<yyyymmdd>`。
3. coding 前用 `git worktree list` 确认 path-to-branch mapping。
4. 保持 active worktrees 数量少，merge 或 handoff 后清理。

## Frontend-First 规则

当任务包含 UI、page、component、interaction、prototype、dashboard 或其他视觉 frontend output 时：

1. 写 frontend code 前必须使用 `web-design-engineer`。
2. 如果已有 design system，必须保持一致。
3. 如果没有 design system，先显式定义视觉规则再构建。
4. 避免 generic filler UI、伪造数据和缺乏意图的组件。
5. 重要 frontend behavior 和 UX constraints 必须写入 `design.md` 或 `specs/**/spec.md`，不能只留在 code 中。

不要把 frontend work 当成“只是 implementation”。必须先经过 design reasoning。

## 执行约束

- 优先做 minimal、scoped changes。
- 不修改无关文件。
- 未检查当前 code 和 specs 前，不虚构 API、data contract 或 abstraction。
- 如果 code 与 OpenSpec artifacts 冲突，先停止并协调 artifacts，或明确提出冲突。
- 当 implementation 发现 scope 或 approach 需要变化时，先更新 artifacts。
- 执行期间保持 `tasks.md` 最新，已完成工作要反映到 checkbox。
- spec review 与 code quality review 必须分开。
- 按阶段加载最小必要上下文，不要一次加载所有 skill 和 docs。

## 多模型与工具协作

当使用 subagent、外部模型、浏览器、检索、代码执行器或其他工具时：

- 主 agent 对最终判断负责；工具输出和 subagent 报告只能作为待复核输入。
- 只有任务独立、目标明确、ownership 清楚时才并行分派。
- 为每个 subagent 或工具调用定义 bounded task、输入、输出格式和不允许触碰的范围。
- 不在 overlapping write scopes 上并行编辑；确需并行时，先明确协调策略。
- 对外部模型给出的事实、API、版本、依赖或安全建议进行来源检查或本地验证。
- 合并 subagent 结果前检查 diff、运行相关 verification，并确认没有偏离 OpenSpec artifacts。

## Verification Gate

报告完成前必须：

1. 运行当前 repo 最强可用 checks，例如 tests、lint、build、typecheck 或 targeted verification。
2. 重新阅读当前 OpenSpec change artifacts，确认 implementation 与 artifacts 一致。
3. 当用户使用中文沟通时，确认 OpenSpec artifacts 正文语言以中文为主，并保留必要 OpenSpec 英文标记。
4. 明确说明仍存在的 unresolved risks、gaps 或 skipped checks。
5. 如果 OpenSpec tooling 可用，先 `verify` 再 `archive`。

## 推荐流程

默认路径：

1. Explore 或 clarify
2. 用 OpenSpec 提出 change
3. Refine specs 与 design
4. 将 `tasks.md` 转成 execution plan
5. 以 Superpowers discipline 实施
6. 验证 code 与 artifact coherence
7. Archive 或 handoff

## Skill 位置

本仓库是 `openspec-delivery` skill 源仓库，workflow skill 位于：

`SKILL.md`

安装到目标项目后，通常位于：

`.agents/skills/openspec-delivery/SKILL.md`
