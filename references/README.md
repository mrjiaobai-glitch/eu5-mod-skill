# eu5-mod-review references 索引

本库由 `F:\SteamLibrary\steamapps\common\Europa Universalis V\game\` 下 **74 个 readme**（72 个 `readme.txt` + `production_methods\__readme.txt` + `setup\countries\00_readme.info`）逐文件提炼而成，只记录 readme 实际声明的内容；未声明的字段一律标注"未在 readme 中说明"，**禁止凭 EU4 经验补充**。

**用法**：审查某类目文件前，先按下方映射加载对应 references 文档核对字段名、类型、取值约束与作用域；再用 SKILL.md 的通用检查清单与实测坑做全面检查。

| 类目（readme 所在目录） | references 文件 | 一句话说明 |
|---|---|---|
| common/advances | `common-advances.md` | 科技：age/requires/allow/potential/unlock_*/modifier_while_progressing |
| common/ai_scripted_expansion_score, ai_scripted_expansion_target | `common-ai_scripted_expansion.md` | AI 扩张评分与目标列表（scope:attacker/target 约定） |
| common/artist_types, artist_work | `common-artist.md` | 艺术家学科（ARTIST_TYPE_NAME_/DESC_）与艺术品（captured/三类 modifier） |
| common/attribute_columns | `common-attribute_columns.md` | select_trigger 列定义（widget/sort/column） |
| common/auto_modifiers | `common-auto_modifiers.md` | 自动修正（category/type 顺序约束、scales_with/limit） |
| common/avatars | `common-avatars.md` | 神祇化身（god/potential/时间/on_activate/三类 modifier） |
| common/building_types | `common-building_types.md` | 建筑（is_foreign/in_empty/max_levels 是 scripted integer/各 trigger 作用域） |
| common/bureaucracies | `common-bureaucracies.md` | 政府官僚制（neutral/positive/negative_modifier、price 引用） |
| common/cabinet_actions | `common-cabinet_actions.md` | 内阁行动（select_trigger 全字段、ability、ai_will_do） |
| common/casus_belli | `common-casus_belli.md` | 战争理由（create/declare/province 作用域、required_peace_treaties、loc 键） |
| common/character_interactions | `common-character_interactions.md` | 角色交互（on_other/own_nation、price/payer/payee、cooldown） |
| common/child_educations | `common-child_educations.md` | 儿童教育（character 作用域 modifier/price/effect） |
| common/country_description_categories | `common-country_description_categories.md` | 国家简介分类（loc 键固定前缀） |
| common/country_interactions | `common-country_interactions.md` | 国家交互（type 枚举、**diplo_chance 键全清单**、GUI action_button） |
| common/country_ranks | `common-country_ranks.md` | 国家等级（ai_level 1–4、两个 cooldown） |
| common/disasters | `common-disasters.md` | 灾难（can_start/can_end、monthly_spawn_chance、fire_only_once） |
| common/diseases | `common-diseases.md` | 疾病（r0/spread 规则/specific_pop_type_effect/local_&national_ modifier 约定） |
| common/employment_systems | `common-employment_systems.md` | 就业系统（priority 大者先） |
| common/estate_privileges | `common-estate_privileges.md` | 阶层特权（estate 引用、can_revoke） |
| common/formable_countries | `common-formable_countries.md` | 可成立国家（level/rule 枚举/required_locations_fraction） |
| common/generic_action_ai_lists | `common-generic_action_ai_lists.md` | 通用行动 AI 分组（actions 列表） |
| common/generic_actions | `common-generic_actions.md` | 通用行动（type 枚举、消息键 PERFORM_/WE_PERFORM_ 等、player_automated_category） |
| common/gods | `common-gods.md` | 神祇（religion/group 两种写法、is_female） |
| common/goods | `common-goods.md` | 商品（category/method 枚举、demand_add/multiply、location_potential 报错） |
| common/government_reforms | `common-government_reforms.md` | 政府改革（major 排他、regnal_names、locked） |
| common/holy_site_types, holy_sites | `common-holy_sites.md` | 圣地类型与圣地（importance 1–5、god/avatar 引用） |
| common/institution | `common-institution.md` | 制度（can_spawn root=location、spread 系列） |
| common/international_organizations + international_organization_land_ownership_rules + international_organization_payments + international_organization_special_statuses | `common-international_organizations.md` | IO 全套：字段、IO/country/location 作用域脚本、土地/支付/特殊状态 |
| common/join_war_rules | `common-join_war_rules.md` | 参战规则（scope:war/first_leader/second_leader） |
| common/laws | `common-laws.md` | 法律与政策（law 容器、policy 覆盖 IO 字段语义） |
| common/levies | `common-levies.md` | 征召（**特化单位必须放文件顶部**） |
| common/movements | `common-movements.md` | 思潮（required_* 门槛、religion/culture 二选一、spread 规则） |
| common/parliament_agendas, parliament_issues, parliament_types | `common-parliament.md` | 议会三件套（type 决定 root 作用域） |
| common/peace_treaties | `common-peace_treaties.md` | 和平条约（blocks_full_annexation/collate_targets/cost/antagonism） |
| common/prices | `common-prices.md` | 价格定义（全字段清单、min/min_scale/max_scale） |
| common/production_methods | `common-production_methods.md` | 生产方式（goods_input/produced/potential 国家作用域） |
| common/recruitment_method | `common-recruitment_method.md` | 招募方式（strength/experience/build_time/army/default） |
| common/regencies | `common-regencies.md` | 摄政（start/end_effect、internally_assigned） |
| common/religious_aspects, religious_factions, religious_focuses | `common-religion.md` | 宗教三件套（aspects country 作用域/factions IO 作用域/focuses 挂接） |
| common/resolutions | `common-resolutions.md` | IO 决议（votes/requires_vote/should_finalize_vote、最后一个 select_trigger 是投票对象） |
| common/rival_criteria | `common-rival_criteria.md` | 宿敌标准（仅 AI 遵守） |
| common/road_types | `common-road_types.md` | 道路（level/pop_movement/proximity/construction_demand） |
| common/scripted_diplomatic_objectives | `common-scripted_diplomatic_objectives.md` | 脚本化外交目标（actor/recipient_trigger、max_allowed） |
| common/scripted_effects | `common-scripted_effects.md` | 脚本化效果（$参数$、**custom_description 键不可同名**） |
| common/scripted_relations | `common-scripted_relations.md` | 脚本化关系（type/relation_type、自动修正与 opinion/trust 偏见键、diplo chances） |
| common/scripted_triggers | `common-scripted_triggers.md` | 脚本化触发器（同 effects 的参数与 custom_description 规则） |
| common/situations | `common-situations.md` | 局势（resolution/voters 引用、on_start/monthly/ending/ended） |
| common/subject_military_stances | `common-subject_military_stances.md` | 附庸军事姿态（全 float 优先级） |
| common/subject_types | `common-subject_types.md` | 附庸类型（各 trigger 作用域不同、type 枚举、annexation 系列） |
| common/tests | `common-tests.md` | 测试用例（success/failure、success_child 按序评估） |
| common/town_rights | `common-town_rights.md` | 城镇权利（allow root=country target=location） |
| common/unit_abilities, unit_categories, unit_formation_preference | `common-unit.md` | 单位能力/类别（**is_garrison 全局唯一**、三组 combat modifier） |
| common/unit_types | `common-unit_types.md` | 单位类型（category/copy_from/limit/combat/impact 地形） |
| common/wargoals | `common-wargoals.md` | 战争目标（内置 type 枚举、war_name 独立 loc 键、attacker/defender cost） |
| in_game/events | `common-events.md` | **事件**：type 五选一、category 三值、ai_chance 用 base/add、选项全字段 |
| in_game/gfx/map/map_modes | `gfx-map_modes.md` | 地图模式（color_mode/map_color/刷新计数器） |
| in_game/gui/filters | `gui-filters.md` | 过滤器（scope/trigger/range/tag 省略=全视图、exclusive_group 全组必写） |
| in_game/gui/panels/disaster, situation | `gui-panels.md` | 灾难/局势面板（type = disaster_panel/situation_panel） |
| in_game/setup/countries（00_readme.info） | `setup-countries.md` | 国家定义（description_category 引用、difficulty 1–5） |
| main_menu/common/achievements | `main_menu-achievements.md` | 成就（possible/happened、须加进 achievement_groups） |
| main_menu/common/static_modifiers | `main_menu-static_modifiers.md` | 静态修正（game_data category 枚举、decaying/remove_if） |
| 审查清单辅助（非 readme 提炼） | `audit-ids.md` | SKILL.md 第 5 节 ID 目录与核对规则（源自实测经验），审查引用类 ID 时加载 |
| 实测经验（非 readme 提炼） | `blades-and-thrones-2026-08.md` | 2026-08《刀锋与王座》实测重大坑：country_has_estate 恒真、军队国碎国、num_forts 量级、EU4 语法混入、社会价值轴 progress modifier、存档分析法等 |

## 制作层知识库（eu5-mod-maker 并入，全部基于游戏本体实查）

**与字段库的分工**：上表是**字段权威**（某类目有哪些字段）；下表是**制作层**（文件在哪、怎么组织、怎么落地、怎么测试）。制作某类目内容时：先看 `systems-map.md` 找入口文件与 readme，再加载上方对应字段文档核对字段，最后照原版样例写。

| 文档 | 内容 | 主要依据 |
|---|---|---|
| `game-layout.md` | 游戏本体四区结构地图（in_game / loading_screen / main_menu / dlc 各自含什么） | `game\` 顶层实查 |
| `mod-skeleton.md` | mod 骨架：metadata.json 注册、目录组织、文件命名与加载序、本地化放置 | 本机 `mod\刀锋与王座\` 实查 + `game\dlc\D008_fate_of_the_phoenix\` |
| `merging.md` | 文件合并体系：INJECT/REPLACE/REPLACE_OR_CREATE/INJECT_OR_CREATE、on_action 合并语义、事件不可合并、顺序敏感块 | `on_action\on_actions.info` + 实测 |
| `event-making.md` | 事件**制作**模板：全字段速查 + volcano/qa_debug 真实样例 + 触发方式 | `in_game\events\readme.txt`、`volcano_events.txt`、`debug\qa_debug.txt`（字段权威另见 `common-events.md`） |
| `scripting-core.md` | 核心脚本体系：script_values 公式（_script_values.info 全文要点）、scripted_effects/triggers $参数$、on_action 定义全键、作用域与变量 | `script_values\_script_values.info`、`scripted_effects\readme.txt`、`on_action\on_actions.info` |
| `systems-map.md` | common 类目全地图：~160 个类目分组、入口文件、readme 有无标注 | `in_game\common\` 全目录实查 + 73 个 readme 位置 |
| `defines.md` | defines 体系：00_defines.txt 的 N 块索引（NGame/NCombat/NPop/NUnit/NWeather…）、graphic/jomini defines、game_rules 覆盖 defines | `loading_screen\common\defines\` 实查 |
| `localization.md` | 本地化制作：BOM、l_english 头、键前缀、占位符、语言文件真实位置与镜像 | `main_menu\localization\english\` 实查（键前缀全表另见 `loc-keys.md`） |
| `testing.md` | 游戏自带测试系统（common/tests）、error.log 排错（Script location）、debug 事件模板、观察者验证 | `common\tests\readme.txt`、`events\debug\qa_debug.txt` |
| `pitfalls.md` | 实测坑速查：EU4→EU5 语法对照、作用域/变量/合并/灾难重复坑、本库翻阅观察 | `eu5-mod-review` 实测 + 翻阅观察 |

## 铁律

1. **一切以游戏本体为准**：写任何词条前先 grep 游戏本体确认存在/用法，禁止凭 EU4 记忆补。
2. **EU5 ≠ EU4**：无 ROOT/PREV、无 event_target、无 `KEY:0`、选项无 weight、作用域词 root/prev/this。
3. **字段不确定 → 查 readme**：73 个 `readme.txt` + `_script_values.info` + `on_actions.info` + `_game_rules.info` 是官方权威说明，都在游戏本体里。
4. 制作完成后用本 skill 的审查流程自检。
