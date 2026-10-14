# API Reference / File Index

Read this file first when using the Skill.

## Reading Order

0. `auto-decision-chain-qc.md` — establish automatic decision locks, minimum necessary question rules, full-chain lock QC, and completion gates.
1. `drama-type-specs.md` — classify long-form / medium-form / short-form drama and confirm episode-duration locks.
2. `structure-formulas.md` — calculate act boundaries, anchor episodes, hook density, payoff cadence, and finale placement.
3. `episode-templates.md` — write or inspect single episodes according to drama type.
4. `optimization-diagnosis.md` — diagnose and repair existing scripts.
5. `qc-checklist.md` — final validation before delivery.

## Automatic Decision Principle

Default to automatic decision-making when evidence exists. If a value can be derived from user text, filename, title, source body, context, or computable facts, lock it with evidence and continue. Ask only when a missing value would change plot, relationships, ending, total episodes, drama type, target duration, dialogue preservation, output topology, or another irreversible creative choice. Low-risk technical work such as format cleanup, counting, duration estimation, shot recommendation, segment QC, chain QC, and footer removal must auto-run.

## Format Normalization Reference

For “统一格式规范优化剧本” / format-only normalization, read `format-normalization.md` before `optimization-diagnosis.md`. This task only normalizes numbering, shot headers, dialogue prefixes, inner monologue prefixes, and voice-over prefixes; it must not change original plot content.

## 台词时长与分镜数量规划索引

- `dialogue-shot-duration-planning.md`：根据台词字数、语速、表演留白、读屏、动作复杂度和节奏系数计算时长，再决定推荐分镜数量；包含分段式 QC、连锁式 QC 和全文完成度 QC。


## 自动抉择与连环锁 QC 索引

- `auto-decision-chain-qc.md`：所有任务的第 0 个读取文件。用于判断何时自动锁定、何时弹窗，并确保 intake → structure → character → episode → script → optimization → format → duration_shot → final_qc 的全链路功能不被跳过。
