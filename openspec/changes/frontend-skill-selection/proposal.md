# Frontend Skill Selection

## Why

当前 `Frontend-First` 规则只要求在写 frontend code 前使用 `web-design-engineer`，但没有区分设计判断与实现落地两层。对于需要先定视觉方向、再做具体实现的任务，agent 可能只触发单一 skill，导致设计判断和落地实现之间缺少明确分工。

## Scope

- 在 `SKILL.md`、`AGENTS.md` 和 `CLAUDE.md` 中补充前端 skill 选择顺序。
- 明确 `ui-ux-pro-max` 用于前置 UI/UX 设计判断，`frontend-design` 用于前端实现和具体界面落地。
- 保留 `web-design-engineer` 作为前端工作入口的协调层。

## Non-Goals

- 不修改任何实际前端产品代码。
- 不重写 `ui-ux-pro-max` 或 `frontend-design` 本体。
- 不改变其他 OpenSpec 或 Superpowers 规则。

## Acceptance Criteria

- `Frontend-First` 规则能区分前置设计判断和实现落地。
- `AGENTS.md` 与 `CLAUDE.md` 提示前端工作时应如何按场景选择 `ui-ux-pro-max`、`frontend-design` 和 `web-design-engineer`。
- 规则表达简短，避免复制完整 skill 文档。
