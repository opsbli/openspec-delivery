# Failure Recovery

当 workflow 已经开始漂移，而主 `SKILL.md` 不足以快速处理 failure mode 时，使用本参考。

## 1. Implementation 偏离 Specs

### Symptoms

- code 看起来合理，但不匹配 `specs/**/spec.md`
- task 被标记完成，但 acceptance 仍然模糊
- scope 被“顺手小改”扩大，且从未被批准

### Recovery

1. 重新读取 `proposal.md`、`design.md`、`specs/**/spec.md` 和 owner task。
2. 补充或收紧 task DoD。
3. 在 active execution plan 顶部重申 non-goals。
4. 移除或 defer 未批准 scope。
5. 继续前重新运行 spec review。

### Prevention

- 将 non-goals mirror 到 `tasks.md`
- 保持 tasks narrow
- review against specs，而不是 implementation intent

## 2. TDD 卡住

### Symptoms

- 想不出干净的 failing test
- tests 依赖 DB、network 或 UI state，稳定性不足
- implementer 想“先 build 再说”

### Recovery

1. 继续拆小任务。
2. 从 pure validation、mapping 或 state-transition logic 开始。
3. 把 I/O 放到可替换或可 mock 的 seams 后面。
4. 只为一个 behavior 添加一个 failing test。
5. 继续前添加一个 boundary case。

### Prevention

- 将 tasks 拆到 2-5 分钟颗粒度
- 避免一个 task 混合多个 behaviors
- integrations 周围优先使用 contract tests

## 3. Worktree 使用混乱

### Symptoms

- branch 与 directory mapping 不清楚
- 工作发生在错误 worktree
- 多个 worktrees 看起来指向同一个 change

### Recovery

1. 运行 `git worktree list`。
2. 将每个 worktree 映射到一个 active change。
3. 统一命名，例如 `../<repo>-feature-<change>`。
4. 关闭或移除 stale worktrees。
5. 再次编辑前确认 active path。

### Prevention

- active worktrees 保持少量
- 每个 meaningful active change 使用一个 worktree
- 必要时将 branch/path mapping 写入 active plan

## 4. Verify 后期失败

### Symptoms

- lint、build、tests 或 typecheck 在接近结束时失败
- `verify` 在大部分 implementation 完成后报告 requirement gaps

### Recovery

1. 将每个 failure 映射回 owner task。
2. 先修便宜问题：lint、types、小 tests。
3. 再修 structural 或 integration failures。
4. 为修复的 gap 添加 regression check。
5. 报告进展前重新运行 verification。

### Prevention

- 持续运行 lightweight verification
- 不要批量堆积太多 unchecked tasks

## 5. Spec Review 与 Code Review 冲突

### Symptoms

- 一轮 review 认为 behavior 正确
- 另一轮 review 认为 structure 不可接受
- 因为两类讨论混在一起导致进展停滞

### Recovery

1. 先解决 spec compliance。
2. 将 quality concerns 分离到第二轮。
3. 除非 specs 或 design 已更新，否则 refactors 限定为 behavior-preserving changes。
4. 如果分歧暴露 deeper architecture issue，记录到 `design.md`。

### Prevention

- 始终保持两阶段 review
- 不混合 product scope 与 code-quality opinions
