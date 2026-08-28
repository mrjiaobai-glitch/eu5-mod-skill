# 审查清单辅助：本地化键（loc 键）规则

> 本文件不是 readme 提炼内容，而是 SKILL.md 第 6 节检查清单的按需加载辅助表（源自人工实测经验，含与 readme 声明的交叉核对）。审查本地化前加载。

## 事件键

`<namespace>.<id>.title / .desc / .a / .a.tt`

## 名称即 loc 键（顶层键名 = loc 键，漏一个显示 raw key）

- static_modifier → `STATIC_MODIFIER_NAME_<名>` + `STATIC_MODIFIER_DESC_<名>`（前缀形式；category 枚举见 main_menu-static_modifiers.md）
- auto_modifier → `AUTO_MODIFIER_NAME_<名>`
- casus_belli → 原版常见 `cb_<名>` + `cb_<名>_desc`；readme 声明键为 `<id>` / `<id>_PROV` / `<id>_desc`——**以原版 loc 实际键为准**
- 自定义外交关系 → `<名>_relation` + `<名>_relation_desc`（模板 food_access_relation）
- wargoal 的 `war_name = "XXX"` → 独立 loc 键（如 EXPEL_LANDLESS_WAR_NAME）
- reform / subject_type / estate / law / societal_value / AI 人格 / disaster / regency / scripted_country_names 块名 → 顶层键即 loc 键（subject_type 另需 `LEAD_<名>` / `AM_<名>`）
- artist_type → `ARTIST_TYPE_NAME_<名>` + `ARTIST_TYPE_DESC_<名>`
- country_description_category → `country_description_category_name_<键>` + `country_description_category_desc_<键>`
- generic_action / resolution → `<action_tag>` + `<action_tag>_desc`（无前缀）；消息键 `PERFORM_<Key>_ACTION` / `WE_PERFORM_<Key>_ACTION` / `OTHER_PERFORMS_<Key>_ACTION` / `ACTION_<Key>_PERFORMED_ON_US`

## 不需要 loc 的类型（不要误报）

- modifier 类型名（modifier_type_definitions，引擎处理）
- rebel demand 名（vanilla 全无）

## 占位符与跨引用

- 占位符配对：`[Root.GetName]` 方括号、`[Root.GetVariable('x').GetValue|V0]`、`$KEY$` 引用存在。
- 跨 mod 引用原版/1644 的 loc 键（如 civil_war_game_over）→ 标注"外部依赖"，不报错。
