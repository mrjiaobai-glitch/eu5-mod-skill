# 审查清单辅助：词条与 ID 存在性目录

> 本文件不是 readme 提炼内容，而是 SKILL.md 第 5 节检查清单的按需加载辅助表（源自人工实测经验）。审查引用类 ID 前加载。

## 引用类 ID 目录

modifier、building、government_reform、subject_type、estate_type、estate_privilege、law、societal_value_type、ai_personality、disaster_type、pop_type、relation_type、casus_belli、unit_type、advance、war_goal_type、peace_treaty、special_status、payment、production_method、parliament_type、resolution

## 核对规则（零幻觉）

- 引用类 ID：先查原版 `game\in_game\common\<类目>\` 与 mod 自身 `common\<类目>\`，**存在才通过**；查不到标 `[存疑]`。
- 效果/触发器词条：引擎内置、无全表——明显笔误（重复字符/乱码/大小写错乱）→ 错误；拼写疑似但无法证实 → `[存疑]` + 最接近候选，不断言。
- 脚本化内容：`scripted_effects` / `scripted_triggers` / `on_action` 引用在 mod 或原版目录内可查；scripted effect/trigger 的 `$参数$` 未传会刷大量 missing 报错。
