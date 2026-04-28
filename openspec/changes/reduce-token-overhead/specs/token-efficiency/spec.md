## ADDED Requirements

### Requirement: Token 高效执行

`SKILL.md` SHALL 定义按任务规模选择最小可行流程的规则，`AGENTS.md` 和 `CLAUDE.md` SHALL 提供简短入口提醒。

#### Scenario: Tiny task

- **WHEN** 任务是单文件、非行为变更或极小文案/格式调整
- **THEN** agent SHALL 跳过 OpenSpec artifacts
- **AND** agent SHALL 只执行定位、编辑、最小验证和简短总结

#### Scenario: Small task

- **WHEN** 任务只涉及 1-2 个文件的小范围调整
- **THEN** agent SHALL 使用 compact OpenSpec artifacts
- **AND** agent SHALL 避免重复项目背景或无关章节

#### Scenario: Context loading

- **WHEN** agent 需要读取上下文
- **THEN** agent SHALL 优先使用 `rg`、文件列表和 targeted snippets
- **AND** agent SHALL 避免默认读取完整文件、完整 references 或完整执行栈

#### Scenario: Entry files

- **WHEN** agent 读取 `AGENTS.md` 或 `CLAUDE.md`
- **THEN** 入口文件 SHALL 提醒 agent 先判断任务规模
- **AND** 入口文件 SHALL 指向 `SKILL.md` 作为完整 token 预算规则来源

#### Scenario: Final response

- **WHEN** agent 汇报完成结果
- **THEN** agent SHALL 默认只说明核心文件、关键变化、verification 结果和剩余风险
