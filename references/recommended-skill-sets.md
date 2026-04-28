# 推荐 Skill Sets

选择当前任务需要加载多少 Superpowers process 时，使用本参考。

优先使用小而准确的 active set，不要一次加载所有 skill。

## Minimal Set

适用于 scope 清晰的小变更。

- `superpowers:test-driven-development`
- `superpowers:verification-before-completion`

## Standard Set

适用于多数日常 feature 和 bugfix work。

- `superpowers:test-driven-development`
- `superpowers:verification-before-completion`
- `superpowers:writing-plans`
- `superpowers:requesting-code-review`
- `superpowers:systematic-debugging`

## Extended Set

适用于更大或风险更高、需要更多 process structure 的变更。

- `superpowers:brainstorming`
- `superpowers:using-git-worktrees`
- `superpowers:writing-plans`
- `superpowers:subagent-driven-development`
- `superpowers:test-driven-development`
- `superpowers:requesting-code-review`
- `superpowers:verification-before-completion`
- `superpowers:finishing-a-development-branch`

## 选择 Heuristics

### Small Change

使用：

- OpenSpec quick path
- Minimal Set 或 Standard Set

典型示例：

- small bugfix
- narrow endpoint adjustment
- one-component UI correction

### Medium Change

使用：

- OpenSpec full artifact set
- Standard Set

典型示例：

- 跨少量 modules 的 feature
- moderate frontend flow
- 有明确 regression risk 的 change

### Large Change

使用：

- OpenSpec full artifact set 或多个 coordinated changes
- Extended Set

典型示例：

- multi-day feature
- cross-cutting refactor
- 可 parallelize 的 implementation plan

## Frontend Addition

当任务影响 UI 时，始终额外加入：

- `web-design-engineer`

这个 frontend 要求独立于 Superpowers selection，不应被跳过。
