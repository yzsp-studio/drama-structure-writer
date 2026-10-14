# 自动抉择与全链路连环锁 QC

## 用途

本文件把 `drama-structure-writer` 从“默认弹窗确认型”升级为“自动抉择型 + 最小必要提问型”，同时保证所有能力不会被自动抉择跳过。注意：叙事方式抉择是本规则的例外，必须按 `narrative-method-selection.md` 弹窗选择。

核心原则：

1. 能从用户明说、文件名、原文标题、正文内容、已有上下文或可统计事实得出的信息，必须自动锁定，不得反复问用户。
2. 自动抉择不是跳步。每一次自动锁定都必须写入 `decision_lock`，并进入后续 QC。
3. 只有会改变剧情、人物关系、结局、总集数、剧种、单集目标时长、对白保留策略、输出拓扑或不可逆创作方向的事项，才允许弹窗。叙事方式会改变信息释放和观看路径，因此始终属于必须弹窗事项，除非用户已明确指定。
4. 格式规范化、台词统计、时长估算、推荐分镜数量、编号连续性、长文分段 QC、链路承接 QC 属于低风险技术执行，默认自动执行。
5. 若证据不足但继续执行不会改剧情，使用 `auto_inferred` 或 `待确认` 标记，不得中断长文处理。

## 自动抉择优先级

```text
decision_priority:
  1_user_explicit: 用户明确说出的内容，直接锁定。
  2_source_strong_evidence: 文件名、标题、正文反复出现的强证据，自动锁定。
  3_computable_fact: 可从全文统计得出的事实，如集数、镜头数、台词量、现有结尾，自动锁定。
  4_task_safe_default: 不改变剧情的执行项，如格式、QC、时长估算，自动执行。
  5_recommended_default: 用户说“你判断 / 继续 / 按推荐 / 你定”时使用推荐方案，并标注 user_authorized_default。
  6_blocking_unknown: 会造成不可逆剧情或结构选择的未知项，才弹窗。
```

## 何时必须自动执行

以下情况不得弹窗，直接执行并记录 evidence_source：

- 统计全文总集数、镜头数、台词字数、VO / OS / 对白数量。
- 判断现有稿件是否有结尾、是否断句、是否重复、是否有平台尾巴。
- 计算台词时长、推荐分镜数、超载镜头、拆镜建议。
- 统一明显格式，如 VO 标注、空行、连续编号、平台尾巴删除。
- 已有剧本优化中，先诊断、再对不改剧情的错别字、断句、重复句、格式问题进行修复。
- 长文处理时分段、分页、建立段落范围、生成 segment_qc / chain_qc / full_document_qc。

## 何时必须弹窗

只有以下事项缺失或冲突时弹窗：

- 选择或改变叙事方式：线性推进、倒叙、插叙、平行叙事、交叉剪辑、蒙太奇、多线群像、限制视角、不可靠叙述、章回单元、环形结构、伪纪录 / 采访式或混合方式。
- 改变剧种、总集数或目标单集时长。
- 改变结局走向、人物生死、CP / 父子 / 师徒 / 阵营关系。
- 删除或重写用户要求保留的对白。
- 新增重大剧情事件、反派身份、核心设定、世界观规则。
- 把原本单集稿改成整季、把诊断改成重写、把文本稿改成视频生产链路。
- 将推荐分镜数落地为新增 / 拆分镜头，且会改变剧情信息呈现顺序或新增画面内容。

## decision_lock 必填格式

每个阶段开始前必须建立或继承锁：

```yaml
decision_lock:
  stage_id: intake | structure | character | episode | script | optimization | format | duration_shot | final_qc
  locked_values:
    drama_type:
      value:
      source: user_explicit | source_strong_evidence | computable_fact | auto_inferred | user_authorized_default | pending
      evidence:
    total_episodes:
      value:
      source:
      evidence:
    target_episode_duration:
      value:
      source:
      evidence:
    task_mode:
      value:
      source:
      evidence:
    dialogue_preservation:
      value:
      source:
      evidence:
    narrative_method:
      value:
      source: user_explicit | question_selected | user_authorized_default
      evidence:
      recommendation_reason:
  unresolved_blocking_items: []
  unresolved_nonblocking_items: []
  next_action: auto_continue | ask_user | stop_report
```

## 全链路连环锁 QC

`chain_qc` 不能只用于分镜长文。每个阶段都必须继承上一阶段的锁与未解决项。

```yaml
chain_lock_qc:
  current_stage:
  previous_stage:
  inherited_locks:
    drama_type: PASS | FAIL | N/A
    total_episodes: PASS | FAIL | N/A
    target_episode_duration: PASS | FAIL | N/A
    task_mode: PASS | FAIL | N/A
    dialogue_preservation: PASS | FAIL | N/A
    plot_change_permission: PASS | FAIL | N/A
  required_reference_read:
    expected: []
    read: []
    status: PASS | FAIL
  required_outputs_from_previous_stage:
    expected: []
    present: []
    status: PASS | FAIL
  skipped_capability_check:
    drama_type_formula: PASS | FAIL | N/A
    character_conflict: PASS | FAIL | N/A
    episode_plan: PASS | FAIL | N/A
    optimization_diagnosis: PASS | FAIL | N/A
    format_normalization: PASS | FAIL | N/A
    dialogue_duration_shot_count: PASS | FAIL | N/A
    segment_qc: PASS | FAIL | N/A
    chain_qc: PASS | FAIL | N/A
    full_document_qc: PASS | FAIL | N/A
  unresolved_items_carried_forward: []
  chain_status: PASS | FAIL
```

## 阶段入口锁

### intake 阶段

- 自动从用户话语、文件名、标题、正文判断任务类型。
- 可统计的剧集数、镜头数直接统计，不问用户。
- 若用户没有说目标时长，但已有稿件能估算实际时长，则自动标记为 `computed_current_duration`，不得当成用户确认目标时长。

### structure 阶段

- 读取 `drama-type-specs.md`、`structure-formulas.md` 与 `narrative-method-selection.md`。
- 在计算幕式 / 阶段边界前，若叙事方式未由用户明确指定，必须弹窗给出推荐叙事方式和备选项。
- 已有完整稿件：先按现有集数和剧情锚点诊断，不强行套模板。
- 若原文剧种与公式推荐不一致，先标注冲突，不自动改剧种。

### character 阶段

- 自动提取人物关系与成长线。
- 不改变关系，只检查是否闭合。
- 发现关系断链但修复会改剧情时，弹窗。

### episode / script 阶段

- 自动检查每集钩子、冲突、转折、结尾承接。
- 只要不改剧情，可自动顺句和标记问题。
- 改集数、增删场次、改结尾必须弹窗。

### optimization 阶段

- 必须先诊断。
- 明显错句、重复句、平台尾巴、标注混乱可自动修复。
- 重大剧情修复必须弹窗。

### format 阶段

- 读取 `format-normalization.md`。
- 自动执行编号、字段头、对白 / OS / VO 规范化。
- 原文缺失且无法安全补全的字段标注 `待确认`，不中断全文。

### duration_shot 阶段

- 读取 `dialogue-shot-duration-planning.md`。
- 必须逐段统计台词字数、计算时长、推荐分镜数。
- 如果只是出“建议表”，自动执行。
- 如果要把推荐分镜数落地为新增镜头并可能改变画面内容，弹窗确认。

### final_qc 阶段

- 读取 `qc-checklist.md`。
- 必须输出 `full_chain_lock_qc`，确认从 intake 到 final_qc 没有跳过应执行能力。

## 全文完成声明门槛

只有同时满足以下条件，才能说“已完整完成”：

```yaml
completion_gate:
  all_required_references_read: true
  decision_lock_written_for_each_stage: true
  chain_lock_qc_passed_for_each_stage: true
  no_required_capability_skipped: true
  segment_qc_passed_when_long_document: true
  chain_qc_passed_when_long_document: true
  full_document_qc.final_status: PASS
```

若任一项失败，只能说“已完成到某阶段 / 某些项待补”，不得冒充全文完成。


