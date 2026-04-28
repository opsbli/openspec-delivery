---
name: openspec-delivery
description: "Use when / 用于：项目需要以 OpenSpec 作为 spec 与 traceability layer，并以 Superpowers 作为 execution 与 quality-gate layer；适用于 feature、bugfix、refactor、frontend delivery，以及 coding 前必须维护 proposal/design/specs/tasks artifacts 的变更。"
---

# OpenSpec Superpowers Workflow

用两层机制驱动交付：

- `OpenSpec` 定义 change record、scope、acceptance behavior 和 archive trail。
- `Superpowers` 约束执行方式：brainstorming、planning、TDD、debugging、verification 与 parallelization discipline。

把 OpenSpec 当作“做什么”的 source of truth，把 Superpowers 当作“怎么做”的 source of truth。

这套组合主要防止三类失败：

- 没有 acceptance boundaries 的模糊 implementation
- 快速 coding 后偏离 specs
- 看起来完成但缺少 verification evidence

## 语言策略

当用户使用中文沟通时，以下内容默认使用中文：

- workflow explanations
- design proposals
- implementation plans
- review output
- verification summaries
- user-facing guidance

不要翻译：

- code
- commands
- file paths
- identifiers
- `proposal.md`、`design.md`、`tasks.md`、`spec.md` 等标准 artifact 文件名

OpenSpec 文档正文应以中文为主，但保留 OpenSpec 解析需要的英文标记，例如 `## ADDED Requirements`、`### Requirement:`、`#### Scenario:` 和 `SHALL`。

### Artifact 语言检查（硬性要求）

当用户使用中文沟通时，创建或更新 OpenSpec artifacts 后必须执行 artifact language review：

1. `proposal.md`、`design.md`、`tasks.md` 和 `specs/**/spec.md` 的说明正文必须以中文为主。
2. 只保留 OpenSpec 解析、文件名、命令、代码、identifier 和行业通用术语所需的英文。
3. `specs/**/spec.md` 必须保留 OpenSpec 结构标记，例如 `## ADDED Requirements`、`### Requirement:`、`#### Scenario:`、`SHALL`、`WHEN`、`THEN`、`AND`。
4. 在 completion 前，必须重新读取本次 change 的 OpenSpec artifacts，确认正文语言符合本节要求。
5. 如果发现 artifact 正文默认成英文，先修正 artifacts，再继续 implementation 或 final response。

## 语言锚定（硬性要求）

The anchoring instruction is a self-contained addition to the system prompt.

收到用户在新会话中的第一条消息后，在执行任何 tool call、文件操作或任务推理之前，必须先用中文写一段简短总结：

1. 用户要你做什么
2. 你计划用什么步骤完成
3. 你还缺什么信息

这段中文必须是 agent 的第一个主动输出。完成这段输出后，才能开始执行具体任务。

## 适用范围

当任务不是一次性 trivial edit，并且可能改变 behavior、architecture、UX、tests 或 production code 时，使用本工作流。

只有以下情况可以跳过完整工作流：

- 纯解释，没有 code 或 artifact changes
- 不影响 behavior 的极小文案或格式修改
- 用户明确要求覆盖此流程

## 运行模型

按以下顺序执行：

1. 创建新 artifacts 前，先检查是否已有 `openspec/changes/<change-id>/`。
2. 如果请求仍是想法阶段，先创建或 refine OpenSpec change。
3. 如果请求指向已有 change，先读取 `proposal.md`、存在时的 `design.md`、所有 `specs/**/spec.md` 和 `tasks.md`，再编辑 code。
4. 保持 proposal、design、specs、tasks 职责分离：
   - `proposal.md` 说明 why、scope 和 non-goals
   - `design.md` 说明关键 technical 与 UX decisions
   - `specs/**/spec.md` 定义 externally observable behavior
   - `tasks.md` 定义 execution checklist
5. 将 `tasks.md` 转换为 implementation plan。
6. 按下方映射选择必要的 Superpowers skills。
7. 先验证 code，再验证 artifact coherence，最后 close 或 archive change。

## Superpowers 映射

按任务形态选择 execution skill：

- Feature design、product shaping 或 implementation path 不明确：先用 `superpowers:brainstorming`。
- 需要隔离 workspace 的 scoped feature work：用 `superpowers:using-git-worktrees`。
- 多步骤 implementation：编辑 code 前用 `superpowers:writing-plans`。
- 从 approved plan 执行多任务：当任务独立且 ownership 明确时用 `superpowers:subagent-driven-development`。
- Feature 或 bugfix implementation：用 `superpowers:test-driven-development`。
- Bug investigation、flaky behavior 或 regression analysis：用 `superpowers:systematic-debugging`。
- 边界清楚的独立 workstreams：用 `superpowers:dispatching-parallel-agents`。
- meaningful implementation 后：用 `superpowers:requesting-code-review`，分两轮：先 spec compliance，再 code quality。
- 声称成功前：用 `superpowers:verification-before-completion`。
- verification 后需要 handoff 或 branch wrap-up：如果任务包含分支收尾，用 `superpowers:finishing-a-development-branch`。

如果 repository 有更严格的本地指令，同时遵守本地指令。

## OpenSpec 规则

proposal 阶段：

1. 创建聚焦的 change id。
2. 先写 `proposal.md`。
3. 当 architecture、API shape、UX behavior、data flow 或 migration logic 需要推理时，添加 `design.md`。
4. 围绕 user-visible 或 externally observable behavior 编写 `specs/**/spec.md`。
5. 将 `tasks.md` 写成具体 execution checklist。
6. 保持 non-goals 可见；如果 `proposal.md` 定义了 non-goals，要同步到 `tasks.md` 或 execution plan，防止实现者随意扩大 scope。
7. 当用户使用中文沟通时，完成 artifacts 初稿后立即执行 artifact language review，确保正文以中文为主。

具体 artifact 示例见 [references/example-change.md](references/example-change.md)。

implementation 阶段：

1. 读取当前 change 的所有可用 artifacts。
2. 不要只凭记忆或 chat summary 写 code。
3. 把 `tasks.md` 当成 executable control，而不是普通 checklist。
4. 执行时持续更新 `tasks.md`。
5. 如果 implementation 改变了预期 behavior 或 scope，先更新相关 OpenSpec artifact，再继续。

## 任务拆分规则

implementation 前，将 broad work 拆成 bite-sized tasks：

- 可行时目标为 2-5 分钟任务。
- 每个任务对应一个 observable behavior 或一个 narrow code change。
- 当失败风险不低时，每个任务写清短 DoD。
- 理想情况下，每个任务都暗含：
  - 一个 failing test
  - 一个 minimal implementation step
  - 一个 verification point

如果任务太宽，无法清晰测试，先拆分再 coding。

## Worktree 策略

当 repository 和 tooling 支持时，非平凡变更使用 worktrees。

规则：

1. 一个 meaningful change 对应一个明确的 worktree 和 branch。
2. worktree 使用可预测命名，例如 `../<repo>-<change>-<yyyymmdd>` 或 `../<repo>-feature-<change>`。
3. coding 前用 `git worktree list` 确认 active mapping。
4. active worktrees 保持少量，merge 或 handoff 后清理。

worktree 的目的主要是减少 context pollution 和 cross-change mistakes，不是给 tiny edit 增加仪式感。

## Frontend 分支

当 change 影响 UI 或 frontend behavior：

1. 写 frontend code 前使用 `web-design-engineer`。
2. 将 design decisions 视为 spec 的一部分，不是 incidental implementation detail。
3. 已有 visual system 时保持一致。
4. 没有 visual system 时，先声明 color、type、spacing、interaction 和 responsive rules。
5. 避免 generic placeholder product decisions、fake data 和 low-intent layouts。
6. 重要 UI decisions 要反映在 `design.md` 或 specs 中，不要只留在 implementation code。

## Review 策略

substantial change 需要两阶段 review：

1. Spec review：
   - 只检查 implementation 是否匹配 `proposal.md`、`design.md`、`specs/**/spec.md` 和 task DoD
   - 识别 requirement gaps、missing behaviors 和 scope drift
2. Code quality review：
   - 检查 structure、maintainability、safety、tests 和 regression risk
   - 除非 code 暴露真实 spec conflict，否则不重新打开 product scope

两种 review mode 必须分开，不要把 requirement gaps 与 style/refactor opinions 混在一起。

## Context 加载策略

按阶段保持 context 窄：

- Proposal/design 阶段：加载 OpenSpec artifacts，不加载完整 execution stack。
- Implementation 阶段：加载 `tasks.md`、相关 OpenSpec artifacts，以及必要的 Superpowers skills。
- Verification/archive 阶段：加载 specs、tasks 和 verification evidence，避免不必要的 planning context。

优先使用小而准确的 active skill set，不要加载所有可用 skill。

具体加载预设见 [references/recommended-skill-sets.md](references/recommended-skill-sets.md)。

## 不可妥协约束

- meaningful product 或 code changes 不跳过 OpenSpec artifacts。
- 没有维护 `tasks.md` 时不实现 broad changes。
- 不让 code 静默偏离当前 specs。
- 没有运行最强可用 verification 时不声称完成。
- 不在 overlapping write scopes 上使用 parallel agents。
- 不把 frontend work 当成“只是 code”；先做 design review。

## Failure Recovery

当 workflow 开始漂移时，按以下方式纠正：

- implementation 偏离 specs：补充或收紧 task DoD，重申 non-goals，再重新跑 spec review。
- TDD 卡住：继续拆小任务，从纯 validation 或 mapping logic 开始，把 I/O 隔离到可替换 seam 后面。
- worktree 使用混乱：减少 active worktrees，标准化命名，并在 coding 前确认 branch-to-path mapping。
- verification 后期失败：把每个 failure 映射回 owner task，先修便宜问题，再添加 regression check。
- spec review 与 code review 冲突：先解决 spec compliance，再在 behavior-preserving 边界内讨论 quality changes。

更完整的恢复手册见 [references/failure-recovery.md](references/failure-recovery.md)。

## Completion Gate

报告完成前：

1. 运行相关 tests、lint、build、typecheck 或 targeted checks。
2. 确认 implementation 匹配 `proposal.md`、`design.md`、`specs/**/spec.md` 和 `tasks.md`。
3. 当用户使用中文沟通时，确认 OpenSpec artifacts 正文语言符合“Artifact 语言检查”要求。
4. 明确说明 skipped checks、open risks 或 artifact mismatches。
5. 当 OpenSpec tooling 可用时，优先先 `verify` 再 `archive`。

## 推荐结束状态

一个好的 change 应留下：

- coherent `openspec/changes/<change-id>/` record
- updated code and tests
- verification evidence
- 仍准确描述 shipped behavior 的 artifacts
