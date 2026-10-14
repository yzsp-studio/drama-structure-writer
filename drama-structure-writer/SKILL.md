---
name: drama-structure-writer
display-name-zh: 结构式剧本创作
description: Use when the user wants to create, expand, optimize, diagnose, or standardize Chinese long-form drama, medium-form drama, or short-form drama scripts. This Skill converts ideas, outlines, episode plans, draft scripts, or existing scripts into structurally controlled drama writing outputs. It supports drama-type classification, automatic decision-making with minimum necessary questions, mandatory popup-based narrative-method selection with recommendations, dynamic total-episode formulas, per-episode duration rules, three-act or variant structures, multi-line narrative planning, hook/suspense/payoff design, foreshadowing and prop tracking, character systems, episode-by-episode writing, script optimization, and full-chain lock QC from intake to final_qc. Trigger on: 长剧, 中剧, 短剧, 剧本结构, 剧本创作, 剧本优化, 分集大纲, 单集剧本, 爽点, 钩子, 伏笔, 付费卡点, drama script structure, Chinese drama writing, episode outline, script optimization. Not for final video generation, asset generation, storyboard images, voice, editing, or production pipelines unless another Skill is explicitly invoked.
---

# Drama Structure Writer

## Core Mission

This Skill creates and optimizes structured Chinese drama scripts for three drama types:

- Long-form drama: usually 24-40 episodes, 30-45 minutes per episode.
- Medium-form drama: usually 20-30 episodes, 5-15 minutes per episode.
- Short-form drama: usually 30-100 episodes, 1-3 minutes per episode.

The Skill must not treat the supplied medium-form template as fixed law. The original template is a source reference. All episode counts, act boundaries, anchor episodes, single-episode rhythm, hook density, and payoff cadence must be dynamically adapted by drama type, total episodes, and target episode duration.

## Highest-Priority Interaction Rule: Automatic Decisions and Minimum Necessary Questions

This Skill is automatic-decision-first for technical and derivable facts, but narrative-method decisions are popup-first. At task start, read `references/auto-decision-chain-qc.md`, create a `decision_lock`, and run `chain_lock_qc` at every stage.

If a value can be derived from explicit user text, filename, title, source body, existing context, or computable facts, lock it automatically and continue. Ask only for blocking unknowns that would change plot, relationships, ending, total episodes, drama type, target episode duration, dialogue preservation, output topology, or another irreversible creative direction.

Lock and record evidence for:

1. drama type: long-form / medium-form / short-form;
2. total episodes;
3. target duration per episode;
4. target platform and audience;
5. genre and content boundaries;
6. whether the task is creation, optimization, diagnosis, expansion, or rewriting;
7. whether to preserve existing dialogue verbatim;
8. whether to produce full scripts, outlines, structure tables, or diagnostic reports;
9. narrative method: linear, flashback, montage, parallel, cross-cutting, multi-line ensemble, restricted POV, unreliable narration, chaptered-unit, circular, mock-documentary, or hybrid.

If the user explicitly says “随便 / 你定 / 按推荐 / 继续 / 你判断”, use the recommended option and record `user_authorized_default`. For low-risk technical work such as formatting, counting, duration estimation, recommended shot count, QC, broken-sentence repair, duplicate-line cleanup, or footer removal, auto-continue without popup.
## Highest-Priority Patch: Narrative Method Popup Selection

For any creation, expansion, series plan, episode outline, single-episode script, structural optimization, or rewrite that must decide how the story is told, read `references/narrative-method-selection.md` before structure design. Narrative method choices are not ordinary auto-inferred facts: use a `question` popup to show a recommended option first, explain why it fits, and offer 4-6 relevant alternatives.

This applies to choices such as linear narration, flashback opening, inserted flashbacks, parallel narration, cross-cutting, montage juxtaposition, multi-line ensemble narration, restricted POV, unreliable narration, chaptered-unit structure, circular structure, mock-documentary/interview structure, or hybrids. Only skip the popup when the user already explicitly names the narrative method; then lock it as `user_explicit`. If the user says “你定 / 按推荐 / 继续”, choose the recommended option and record `user_authorized_default`.

The selected `narrative_method` must be written into `decision_lock`, with `recommendation_reason`, information-release rules, and any limits on montage / flashback / cross-cutting so the later script does not silently change storytelling mode.

## Local Template Lock

When this Skill is used, always follow the local files in this directory:

```text
skill_id: drama-structure-writer
skill_display_name: 结构式剧本创作
template_source: local_skill_root
fallback_allowed: false
```

Before working on a stage, read the corresponding reference file from `references/`. Do not rely on memory summaries when a local reference file exists. No stage may skip `decision_lock` or `chain_lock_qc`.

## Quick Routing

0. For every task, first read `references/auto-decision-chain-qc.md`, establish automatic decision locks, and run full-chain lock QC; auto-lock what can be derived, and ask only for blocking unknowns, except narrative-method choices, which must follow `references/narrative-method-selection.md` and use a recommendation popup.
1. User has only an idea or premise → read `references/drama-type-specs.md` and `references/structure-formulas.md`; ask for missing type / episode / duration decisions.
2. User wants a series plan → read `references/structure-formulas.md`; output act plan, anchor episodes, major turns, and episode ranges.
3. User wants a single episode script → read `references/episode-templates.md`; adapt the episode structure to drama type and target duration.
4. User has an existing script to improve → read `references/optimization-diagnosis.md` and `references/qc-checklist.md`; diagnose before rewriting.
5. User asks for full creation from scratch → use all references in this order: drama-type specs → structure formulas → episode templates → optimization diagnosis → QC.

## Standard Workflow

### Stage 1 — Intake and Locks

Confirm or collect:

- project title or working title;
- drama type;
- total episode count;
- target duration per episode;
- genre / subgenre;
- audience and platform;
- core premise;
- creation mode: new writing / optimization / diagnosis / expansion / rewrite;
- preservation rule for existing dialogue and plot.

Automatically lock any value derivable from user text, filename, title, source body, context, or computable facts into `decision_lock`. Ask by popup only for `blocking_unknown` values.

### Stage 2 — Drama-Type Structure

Use dynamic formulas rather than fixed 30-episode examples. Before finalizing act/phase design, confirm the narrative method through the popup rules in `references/narrative-method-selection.md` unless the user explicitly specified it.

For every project, calculate:

- selected narrative method and information-release rules;
- act or phase boundaries;
- first-episode hook target;
- first act turn;
- midpoint crisis or central reversal;
- second act / late-phase turn;
- climax range;
- finale closure;
- recurring hook / payoff density.

### Stage 3 — Character and Conflict System

Every important character must have:

- narrative function;
- social identity;
- core ability;
- core weakness;
- core desire;
- secret or information gap when needed;
- relationship pressure;
- growth arc;
- plot utility and independent moment.

Antagonists must not be stupid tools. Each major antagonist needs understandable motivation, real threat, method, weakness, and a planned defeat / transformation.

### Stage 4 — Episode Plan

For each episode or episode block, define:

- episode position in the whole structure;
- opening hook;
- main conflict;
- line A / line B / antagonist line / support line as appropriate for drama type;
- emotional turn;
-爽点 or payoff;
- foreshadowing planted or recovered;
- ending hook;
- next-episode bridge.

### Stage 5 — Script Writing

Write in Chinese unless the user requests another language. Use clear script format:

```text
场次编号：
场景：
时间：
人物：
动作：
对白：
情绪 / 节奏：
结构功能：
```

Existing user dialogue must be preserved unless the user authorizes rewriting.

### Stage 6 — Optimization and Diagnosis

When optimizing, do not rewrite blindly. First output a diagnosis:

- structure problem;
- hook problem;
- rhythm problem;
- character motivation problem;
- conflict escalation problem;
- foreshadowing / prop tracking problem;
- dialogue problem;
- suggested repair plan.

Then ask for confirmation if the repair changes plot, character relationship, episode count, duration, ending, genre, or tone.

### Stage 7 — QC

Before final delivery, check against `references/qc-checklist.md`. If a required check fails, report the failure and repair the nearest relevant stage.

## Default Deliverables

Choose only the deliverable needed by the user:

- drama structure card;
- episode-count adaptation table;
- act / phase plan;
- character system table;
- episode outline;
- single-episode structure sheet;
- full episode script;
- script diagnosis report;
- rewrite plan;
- optimized script;
- QC checklist.

## Reference Files

- `references/api_reference.md`: file index and reading order.
- `references/auto-decision-chain-qc.md`: automatic decisions, minimum necessary questions, full-chain lock QC, and completion gates.
- `references/drama-type-specs.md`: long-form / medium-form / short-form definitions.
- `references/structure-formulas.md`: dynamic formula rules for episodes, acts, anchors, and density.
- `references/narrative-method-selection.md`: popup-based narrative method selection, recommendation logic, montage / flashback / parallel / cross-cutting choices.
- `references/episode-templates.md`: per-episode structures by drama type and duration.
- `references/optimization-diagnosis.md`: script optimization and repair rules.
- `references/format-normalization.md`: format-only script normalization rules for continuous shot numbering, fixed shot headers, dialogue / OS / VO prefixes, and no-content-change constraints.
- `references/qc-checklist.md`: final quality checks.

## Highest-Priority Patch: Format-Only Normalization

When the user asks for “统一格式规范优化剧本” or format-only normalization, read `references/format-normalization.md`. This mode overrides ordinary script optimization: normalize format only; do not add, delete, reorder, rewrite, reinterpret, or improve original plot content.

Required operations:

- shot numbering starts from `1` and remains continuous;
- every shot group uses `（场次：时段 | 场景参考图 | 景别，机位 | 镜头运镜 | 自然微表情 | 自然体态 |）`;
- spoken dialogue uses `（人物情绪）（开口说道）`;
- inner monologue uses `（人物情绪）（内心独白os）`;
- voice-over uses `（人物情绪）（画外音vo）`;
- if source fields are missing and cannot be safely copied, ask by popup or mark `待确认`; never invent story content.

## Highest-Priority Patch: Dialogue Count, Duration, Shot Count, and Long-Document QC

When a task involves storyboard shots, shot groups, episode scripts, video tasks, voice-over, inner monologue, or spoken dialogue segments, read `references/dialogue-shot-duration-planning.md`.

Fixed execution order:

1. classify dialogue types and count Chinese characters;
2. calculate `duration_calculation` from speech rate, performance pauses, prop reading time, action complexity, and pacing multiplier;
3. decide `recommended_shot_count` from calculated duration and content complexity;
4. then normalize shot numbering, fixed shot headers, dialogue / OS / VO prefixes;
5. for long documents, run `segment_qc` and `chain_qc`;
6. only when `full_document_qc.final_status: PASS` may the agent claim the full document is complete.

Never delete dialogue, speed up speech unnaturally, sacrifice performance pauses, or change plot content to fit a preset shot count or duration. Never degrade later sections into “same as above / omitted / continue similarly”.

## Highest-Priority Patch: Automatic Decisions and Full-Chain Lock QC

For any creation, optimization, diagnosis, normalization, or long-document task, read `references/auto-decision-chain-qc.md`.

Fixed execution order:

1. Create `decision_lock`: automatically lock values derivable from the user, filename, title, source body, context, or computable facts.
2. Ask only for `blocking_unknown`; mark non-blocking unknowns as `待确认` or `auto_inferred` and continue.
3. Before entering each next stage, run `chain_lock_qc` to verify inherited locks, reference files, outputs, unresolved items, and required capabilities.
4. Automatic decisions must not skip capabilities. If a task involves structure, character, episodes, optimization, format, dialogue duration, shot counts, or long-document QC, the matching reference and QC must run.
5. Final delivery must include `full_chain_lock_qc` and `completion_gate`. If any required item is not PASS, do not claim the whole document is fully complete.


