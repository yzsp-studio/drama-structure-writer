# Optimization and Diagnosis

## Do Not Rewrite Blindly

When the user provides an existing script or outline, first diagnose. Do not immediately rewrite unless the user explicitly asks for direct rewriting and the repair does not change major choices.

## Diagnosis Categories

1. Structure: act position, episode position, missing turns.
2. Hook: weak opening, late conflict, unclear stakes.
3. Rhythm: slow middle, repeated beats, no escalation.
4. Character: unclear desire, weak motivation, inconsistent behavior.
5. Antagonist: low threat, stupid decisions, no independent plan.
6. Multi-line narrative: missing antagonist line, repeated subplot, no information gap.
7. Foreshadowing: planted but not recovered, recovered too soon, item tracking broken.
8. Dialogue: exposition-heavy, no subtext, overwritten, not matched to character.
9. Genre promise:爽点 / emotion / suspense / romance / revenge promise not delivered.

## Repair Approval Rule

Use popup confirmation before repairs that change:

- drama type;
- total episodes;
- single-episode duration;
- ending;
- CP relationship;
- protagonist identity or motivation;
- antagonist identity;
- core setting;
- genre tone;
- paid-point placement;
- existing dialogue preservation.

## Repair Output Format

```text
问题：
影响：
推荐修复：
需要用户确认：是 / 否
修复后版本：
```



## 最高优先级补丁：仅格式规范化

如果用户要求“统一格式规范优化剧本”或“只规范格式”，不得执行戏剧性重写。必须读取 `format-normalization.md`，只做机械格式规范：

- 分镜从 `1` 连续编号；
- 固定分镜字段头：`（场次：时段 | 场景参考图 | 景别，机位 | 镜头运镜 | 自然微表情 | 自然体态 |）`；
- 人物台词前缀：`（人物情绪）（开口说道）`；
- 内心独白前缀：`（人物情绪）（内心独白os）`；
- 画外音前缀：`（人物情绪）（画外音vo）`；
- 不得新增或删除原有剧情内容。

