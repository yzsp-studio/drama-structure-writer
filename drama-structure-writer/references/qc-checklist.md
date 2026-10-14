# QC Checklist

## Intake Checks

- [ ] `auto-decision-chain-qc.md` was read first.
- [ ] `decision_lock` exists for intake.
- [ ] Automatically derivable values were locked with evidence instead of repeatedly asking the user.
- [ ] Only true `blocking_unknown` items triggered popup questions.
- [ ] Drama type is confirmed, auto-inferred with evidence, computable, or user-authorized default.
- [ ] Total episode count is confirmed, computed from source, auto-inferred with evidence, or user-authorized default.
- [ ] Single-episode duration is confirmed, computed as current draft duration, marked pending, or user-authorized default; computed current duration must not be mislabeled as a user target.
- [ ] Task mode is clear: creation / optimization / diagnosis / expansion / rewrite.
- [ ] Existing dialogue preservation rule is clear when source text exists.

## Whole-Series Structure Checks

- [ ] Act or phase boundaries are calculated dynamically.
- [ ] Anchor episodes match total episode count.
- [ ] First episode has the strongest setup and hook.
- [ ] Midpoint or central reversal is planned.
- [ ] Late turn and climax range are planned.
- [ ] Finale resolves the core conflict and emotional arc.

## Episode Checks

- [ ] Opening hook appears within the correct time window for the drama type.
- [ ] Episode has a clear conflict escalation.
- [ ] Ending creates a reason to continue.
- [ ] The episode duration can contain the planned dialogue and action.
- [ ] Medium-form episodes use multi-line narrative only when duration supports it.
- [ ] Short-form episodes do not overload too many lines or scenes.

## Character Checks

- [ ] Protagonist has identity, ability, weakness, desire, and arc.
- [ ] Antagonist has motivation, threat, method, weakness, and real agency.
- [ ] Supporting characters have functional roles and at least one independent moment when relevant.
- [ ] Relationship changes are caused by events, not declared without action.

## Hook / Payoff Checks

- [ ] Every episode has a hook.
- [ ] Hook types are not mechanically repeated.
- [ ]爽点 has setup and cost.
- [ ] Paid-point or major reversal cadence fits drama type.

## Foreshadowing and Prop Checks

- [ ] New foreshadowing has a planned recovery.
- [ ] Old foreshadowing is tracked.
- [ ] Important props or evidence have a clear source, location, transfer, and outcome.


## Failure Route

If any required item fails, return to the nearest producer stage:

- intake failure → ask popup;
- structure failure → recalculate formulas;
- episode failure → rewrite episode template;
- character failure → repair character system;
- hook/payoff failure → adjust episode plan;
- foreshadowing/prop failure → update tracking table;
- dialogue failure → rewrite only the affected dialogue unless user authorizes broader rewrite.

## 最高优先级补丁：格式规范 QC

仅格式规范化任务必须检查：

- [ ] 分镜从 `1` 开始连续编号。
- [ ] 每组分镜使用 `（场次：时段 | 场景参考图 | 景别，机位 | 镜头运镜 | 自然微表情 | 自然体态 |）`。
- [ ] 人物台词使用 `（人物情绪）（开口说道）`。
- [ ] 内心独白使用 `（人物情绪）（内心独白os）`。
- [ ] 画外音使用 `（人物情绪）（画外音vo）`。
- [ ] 原有剧情内容、台词含义、场景顺序、人物动作、悬念和结局没有被故意改动。

失败时只修复格式，不得改剧情。


## 最高优先级补丁：台词时长、分镜数量与长文完整性 QC

涉及分镜、镜头组、单集脚本、视频任务、画外音、内心独白或口播段落时，必须检查：

- [ ] 是否读取 `dialogue-shot-duration-planning.md`。
- [ ] 是否统计每句台词的 `dialogue_type` 与 `chinese_char_count`。
- [ ] 是否按语速、表演留白、道具读屏、动作复杂度和节奏系数计算 `duration_calculation`。
- [ ] 是否根据计算时长得到 `recommended_shot_count`。
- [ ] 是否写出 `shot_duration_distribution`。
- [ ] 若超长，是否标注 `split_required` 与 `split_reason`，且没有删台词或加速朗读。
- [ ] 长文是否按段执行 `segment_qc`。
- [ ] 段落之间是否执行 `chain_qc`，分镜编号、对白 ID、剧情尾势和未解决字段没有断链。
- [ ] 是否输出 `full_document_qc`。
- [ ] 后半部分是否没有降级成“同上 / 按前文 / 后续同理 / 略”。

失败时只修复失败段或断链处，不得把已 PASS 的段落压缩重写。


## 最高优先级补丁：自动抉择与全链路连环锁 QC

所有任务最终必须检查：

- [ ] 是否读取 `auto-decision-chain-qc.md`。
- [ ] 每个执行阶段是否有 `decision_lock`。
- [ ] 每个阶段是否执行 `chain_lock_qc`。
- [ ] 用户明说、文件名、标题、正文、上下文、可统计事实是否优先自动锁定。
- [ ] 低风险技术执行项是否自动执行，没有反复弹窗。
- [ ] 弹窗是否仅用于会改变剧情、人物关系、结局、总集数、剧种、目标时长、对白保留策略或输出拓扑的 blocking unknown。
- [ ] `skipped_capability_check` 是否确认结构、人物、分集、优化、格式、台词时长、推荐分镜、segment_qc、chain_qc、full_document_qc 没被跳过。
- [ ] `unresolved_items_carried_forward` 是否从前一阶段传到后一阶段。
- [ ] 最终是否输出 `full_chain_lock_qc` 与 `completion_gate`。

失败路由：

- 自动抉择证据不足但被当成确认值 → 回到 intake 修正 `decision_lock`。
- 阶段能力被跳过 → 回到最近缺失阶段补执行，不得只在最终 QC 口头声明。
- 连环锁断链 → 只修复断链处，并继承已 PASS 阶段，不得压缩重写全文。
- 完成门槛未 PASS → 不得声称“完整完成”，只能报告已完成范围与待补项。
