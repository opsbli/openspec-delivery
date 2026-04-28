# 降低 Skill 执行 Token 开销

## Why

- 小改动新开窗口时上下文消耗过高。
- 当前 workflow 容易默认加载完整 OpenSpec、Superpowers 和 references。
- 用户希望支持分阶段生成、减少重复上下文、简洁输出和增量测试。

## Scope

- 在 `SKILL.md` 增加任务分级、轻量路径和 token 预算规则。
- 优化 OpenSpec、Superpowers、context loading 和 completion 输出策略。
- 同步全局安装版本。

## Non-Goals

- 不删除 OpenSpec + Superpowers 主流程。
- 不重写 references 或 README。
- 不改变 skill 名称和触发描述。

## Acceptance Criteria

- tiny/small 任务有明确轻量路径。
- context loading 明确要求按需读取、避免重复背景。
- final response 默认简短。
- 全局安装目录包含更新后的 `SKILL.md`。
