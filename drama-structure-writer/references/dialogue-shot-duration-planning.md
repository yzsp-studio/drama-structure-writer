# 台词数量、时长与分镜数量规划规则

## 用途

当用户要求创作、优化、规范剧本，并且产物涉及分镜、镜头组、单集脚本、视频任务、口播段落、画外音或内心独白时，必须读取本文件。

本文件把“台词数量 / 字数 → 预计时长 → 推荐分镜数量 → 分段或连锁 QC”的能力同步到 `drama-structure-writer`。

## 最高优先级原则

不得先固定分镜数量再硬塞台词。必须先统计台词，再计算时长，再决定分镜数量和分镜分配。

禁止：

- 不得为了压进固定分镜数量而删减台词。
- 不得为了压进固定时长而让角色高速说话。
- 不得为了减少分镜而合并不兼容的情绪转折、动作节拍或信息揭露。
- 不得在仅格式规范化任务中改变原有剧情内容或台词含义。
- 不得只写“按上文 / 同上 / 延续前段”来省略后续分镜计算。

## 台词类型分类

每句文本必须先归类：

```text
dialogue_type: spoken_dialogue | inner_monologue_os | voice_over_vo | prop_reading | silence_or_action
```

说明：

- `spoken_dialogue`：人物开口说出的台词。
- `inner_monologue_os`：内心独白 / OS。
- `voice_over_vo`：画外音 / VO / 旁白。
- `prop_reading`：短信、纸条、合同、证据、屏幕、图纸、牌匾等需要观众读屏的文字。
- `silence_or_action`：无台词但有表演、动作、转身、屏息、抬眸、抢夺、停顿等可拍节拍。

## 语速基准

按中文字数估算，不含标点；英文、数字、外语或复杂名词按实际朗读难度折算。

| 类型 | 推荐语速 |
|---|---|
| 普通清晰对白 | 每秒 4-5 个中文字 |
| 克制文戏、低声质问、压抑旁白 | 每秒 3-4 个中文字 |
| 情绪迟疑、哽咽、儿童、喘息明显 | 每秒 2.5-3.5 个中文字 |
| 快节奏争吵、打断式对白 | 每秒 4.5-5.5 个中文字，但必须保留反应留白 |
| 画外音（VO）说明性叙述 | 每秒 3.5-4.5 个中文字 |
| 内心独白（OS） | 每秒 3-4 个中文字 |

如果用户指定语速、平台、演员风格或情绪状态，以用户明确要求优先。

## 额外节拍时间

除台词朗读时长外，必须加入必要表演和信息节拍：

| 节拍类型 | 推荐时长 |
|---|---|
| 关键对白前后表演留白 | 0.4-1.2 秒 |
| 抬眸、屏息、停顿、转身、反应 | 0.8-2 秒 |
| 证据揭露、道具特写、读屏开始 | 1-3 秒 |
| 文字道具读屏 | 1.5-2.5 秒，长文本需拆分 |
| 抢夺、推开、靠近、后退等连续动作 | 2-4 秒 |
| 空镜或环境压迫建立 | 1-3 秒 |
| 情绪爆点后余震 | 0.8-2 秒 |

## 节奏系数

计算基础时长后，按类型乘以节奏系数：

| 类型 | pacing_multiplier |
|---|---|
| 人物剧情片 / 家庭生活流 / 物哀日常 | 1.15-1.35 |
| 暗黑复仇 / 悬疑惊悚 / 阶层类型片 | 1.05-1.25 |
| 动作 / 警匪 / 追逐 / 打斗 | 0.85-1.10 |
| 史诗 / 仪式 / 大场面 | 1.20-1.50 |
| 竖屏短剧强爽点压缩节奏 | 0.95-1.10 |

若不确定题材节奏，必须弹窗询问或使用 `pacing_multiplier_source: 待确认`。

## duration_calculation 必填字段

每个分镜组、场次片段或视频任务必须先写：

```text
duration_calculation:
  source_dialogue_ids: []
  spoken_text_breakdown:
    - dialogue_id:
      speaker:
      dialogue_type:
      text:
      chinese_char_count:
      speech_rate_chars_per_second:
      estimated_speech_seconds:
      emotion_or_delivery_basis:
  silent_performance_beats:
    - beat:
      estimated_seconds:
      reason:
  prop_reading_time:
    - prop_or_text_id:
      estimated_seconds:
      reason:
  action_complexity_time:
    - action:
      estimated_seconds:
      reason:
  subtotal_seconds_before_multiplier:
  pacing_multiplier:
  pacing_multiplier_reason:
  total_seconds_after_multiplier:
  recommended_duration_seconds:
  recommended_shot_count:
  shot_duration_distribution:
    - shot_no:
      duration_seconds:
      reason:
  split_required: true | false
  split_reason:
```

## 推荐分镜数量公式

分镜数量不是固定值，按计算时长和内容复杂度决定。

基础规则：

```text
recommended_shot_count = ceil(recommended_duration_seconds / average_shot_length)
```

平均镜头长度建议：

| 内容类型 | average_shot_length |
|---|---|
| 强冲突对话、短剧爽点 | 2-3 秒 |
| 克制文戏、情绪交换 | 3-5 秒 |
| 证据读屏 / 道具揭露 | 2-4 秒 |
| 动作追逐 / 打斗 | 1.5-3 秒 |
| 空镜 / 仪式 / 氛围建立 | 3-6 秒 |

调整规则：

- 每个重大信息揭露至少独立 1 个分镜。
- 每个关键情绪反应至少独立 1 个近景或特写分镜。
- 每个重要道具读屏至少独立 1 个分镜。
- 两人对话出现攻防转换时，攻方 / 受方 / 反应方应拆成不同分镜。
- 如果一个分镜同时承载“长台词 + 大动作 + 道具读屏 + 情绪反转”，必须拆分。

## 分段式 QC

“分段式 QC”用于检查每一段是否完整执行。适用于长文、长场次、多分镜、多集剧本。

每一段必须独立通过：

```text
segment_qc:
  segment_id:
  source_range:
  source_dialogue_ids: []
  dialogue_char_count_done: true | false
  duration_calculation_done: true | false
  recommended_shot_count_done: true | false
  shot_numbering_continuous_in_segment: true | false
  fixed_shot_header_present: true | false
  dialogue_prefix_done: true | false
  os_vo_prefix_done: true | false
  no_plot_content_changed: true | false
  unresolved_fields: []
  segment_status: PASS | FAIL
```

任一段 `segment_status: FAIL` 时，不得宣称全文完成。

## 连锁式 QC

“连锁式 QC”用于确保长文从前到后不断链。后一段必须显式继承前一段的编号、剧情尾势、台词映射和未解决字段。

每一段完成后写入：

```text
chain_qc:
  current_segment_id:
  previous_segment_id:
  previous_last_shot_no:
  current_first_shot_no:
  shot_number_chain_status: PASS | FAIL
  previous_unresolved_fields_carried: []
  current_unresolved_fields: []
  dialogue_id_chain_status: PASS | FAIL
  plot_continuity_status: PASS | FAIL
  format_continuity_status: PASS | FAIL
  duration_chain_status: PASS | FAIL
  chain_status: PASS | FAIL
```

连锁要求：

- 当前段第一个分镜编号必须等于上一段最后一个分镜编号 + 1，除非用户明确要求重置。
- 上一段未解决字段必须在当前段继续标记，不得丢失。
- `dialogue_id` 不得重复、跳号或丢失。
- 若上一段有未完成台词、动作尾势、情绪尾势或悬念，当前段必须承接或明确标注“待后续承接”。
- 如果链条中任一段 FAIL，最终交付必须说明失败段并只修复失败段，不得重写已 PASS 段。

## 长文执行完整性防压缩规则

处理长文时，Agent 必须采用“分段执行 + 连锁检查”的方式：

1. 先列出总段数与段落范围。
2. 每段单独执行台词统计、时长计算、分镜数量推荐、格式规范和 segment_qc。
3. 段与段之间执行 chain_qc。
4. 最后输出全文总 QC。

禁止在后半部分降级为摘要、概括、同上、略、照前、保持一致、后续同理。

## 全文总 QC

```text
full_document_qc:
  total_segments:
  passed_segments:
  failed_segments:
  total_dialogue_ids:
  dialogue_ids_covered:
  total_recommended_duration_seconds:
  total_recommended_shot_count:
  shot_numbering_global_continuous: true | false
  all_segments_have_duration_calculation: true | false
  all_segments_have_segment_qc: true | false
  all_links_have_chain_qc: true | false
  no_plot_content_changed: true | false
  final_status: PASS | FAIL
```

只有 `final_status: PASS` 时，才允许说“全文已完整规范 / 完整优化”。

## 与仅格式规范化的关系

如果用户要求“仅格式规范化”：

- 可以计算并标注台词字数、时长和推荐分镜数量；
- 不能因此新增剧情内容；
- 原文缺少景别、机位、微表情、体态时，必须标注 `待确认` 或弹窗询问；
- 不得为了匹配推荐分镜数量而拆改原剧情事件，除非用户确认。
