# Design

## Selection Order

前端任务按三层处理：

1. `ui-ux-pro-max` 负责前置 UI/UX 判断，适合需要确定视觉风格、布局方向、交互模式和设计取舍的场景。
2. `frontend-design` 负责具体前端实现和 UI 落地，适合已经有明确设计方向或要直接编码的场景。
3. `web-design-engineer` 作为现有项目中的前端工作协调入口，用于将前置设计和实现约束接到当前 workflow。

## Rule Shape

规则必须短，不复制 skill 本体。目标是让 agent 在前端任务里知道先用哪个 skill，再把上下文收敛到当前阶段需要的内容。

## Fallback

如果任务已经有明确设计系统或设计约束，agent 可以直接进入 `frontend-design`；如果任务的视觉/交互方向不清晰，应先用 `ui-ux-pro-max` 做判断，再进入实现。
