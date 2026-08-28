# common 类目地图（systems-map）

`in_game\common\` 全部类目（实查 ~160 个），按功能分组。**每类目制作流程**：看本表找入口文件 → read 原版样例 → 加载 eu5-mod-review 对应 references 核对字段 → 照抄改。标注 `[有readme]` 的类目其 `readme.txt` 是官方字段说明（73 个 total）。

## 经济与生产

| 类目 | 用途 | 入口文件（原版） |
|---|---|---|
| goods `[有readme]` | 商品定义（category/method 枚举、demand） | 00_default.txt |
| goods_demand / goods_demand_category | 商品需求分组（建筑/单位维护需求引用） | 建筑维护如 `infantry_maintenance` 定义处 |
| prices `[有readme]` | 价格定义（min/min_scale/max_scale） | 00_default.txt |
| production_methods `[有readme]` | 生产方式（goods_input/produced） | 00_default.txt（另有 `__readme.txt`） |
| building_categories / building_types `[有readme]` | 建筑类别与建筑（level/employment/cost/effect） | building_types\00_default.txt、town_buildings.txt、rural_buildings.txt 等 |
| employment_systems `[有readme]` | 就业系统（平等/先来先得/资本主义） | 00_default.txt |
| town_rights `[有readme]` | 城镇权利（区域特色建筑/修正） | 00_traditions.txt、10_country_specific.txt… |
| town_setups | 城镇初始布局 | 00_default.txt |

## 军事

| 类目 | 用途 | 入口文件 |
|---|---|---|
| unit_categories `[有readme]` | 单位类别（基础属性：frontage/initiative/combat_speed 等） | 00_army_light_infantry.txt … 04_army_artillery.txt、10_navy_galley.txt… |
| unit_types `[有readme]` | 兵种（copy_from 模板、age、upgrades_to、combat/impact 地形） | 00_age_templates_land.txt、2_unlocked_through_tech.txt、3_janissaries.txt |
| unit_abilities `[有readme]` | 部队能力（drill_army、scorch_earth、ransom_prisoners…） | 每能力一个文件 |
| unit_formation_preference `[有readme]` | 阵型偏好 | 00_default.txt |
| levies `[有readme]` | 征召（**特化单位必须放文件顶部**） | 00_default.txt |
| recruitment_method `[有readme]` | 招募方式（strength/experience/build_time） | 00_default.txt |
| subject_military_stances `[有readme]` | 附庸军事姿态（AI 优先级 float） | 00_default.txt |

## 人口与阶层

| 类目 | 用途 | 入口文件 |
|---|---|---|
| pop_types `[无readme]` | POP 类型（promote_to/assimilation_conversion_factor/literacy_impact） | 00_default.txt（nobles/clergy/burghers/laborers/soldiers/peasants/tribesmen/slaves） |
| estates `[无readme]` | 阶层（power_per_pop/tax_per_pop/satisfaction/power 块） | 00_default.txt（1275 行） |
| estate_privileges `[有readme]` | 阶层特权 | nobles_estate.txt、peasants_estate.txt… |

## 政府与政治

| 类目 | 用途 | 入口文件 |
|---|---|---|
| government_types | 政体类型 | 00_default.txt |
| government_reforms `[有readme]` | 政府改革（major 排他、regnal_names） | common.txt、monarchy.txt、country_specific.txt |
| laws `[有readme]` | 法律与政策（law 容器） | 00_common.txt、01_military_laws.txt… |
| bureaucracies `[有readme]` | 政府官僚制 | generic.txt、byz.txt |
| country_ranks `[有readme]` | 国家等级（ai_level 1–4） | 00_default.txt |
| regencies `[有readme]` | 摄政 | 00_default.txt |
| policies | 政策 | 00_default.txt |
| country_description_categories `[有readme]` | 国家简介分类（loc 前缀固定） | 00_default.txt |
| designated_heir_reason / heir_selections / death_reason | 继承/死亡原因词条 | 00_default.txt |
| cabinet_actions `[有readme]` | 内阁行动 | 00_default.txt、promote_religion.txt… |
| chivalric_orders | 骑士团 | 00_default.txt、02_german_societies.txt |

## 文化与人口属性

| 类目 | 用途 | 入口文件 |
|---|---|---|
| cultures / culture_groups | 文化与文化组 | 00_default.txt |
| languages / language_families | 语言与语系 | 00_default.txt |
| ethnicities | 族群 | 00_default.txt |
| traits / trait_flavor | 特质（ruler/cabinet/religious_figure 等） | 00_ruler.txt、06_religious_figure.txt、07_cabinet.txt |
| genes / persistent_dna | 角色外貌基因 | 00_default.txt |
| child_educations `[有readme]` | 儿童教育 | 00_default.txt |
| avatars `[有readme]` | 神祇化身 | 00_default.txt |

## 宗教

| 类目 | 用途 | 入口文件 |
|---|---|---|
| religions / religion_groups | 宗教与宗教组 | christian.txt、muslim.txt、00_religion_groups.txt |
| religious_aspects / religious_factions / religious_focuses / religious_figures / religious_schools | 宗教五件套 | common.txt、calvinist.txt、sunni.txt… |
| holy_sites / holy_site_types `[有readme]` | 圣地与类型 | 00_default.txt |
| gods `[有readme]` | 神祇（religion/group 两种写法） | hellenism.txt… |

## 外交与战争

| 类目 | 用途 | 入口文件 |
|---|---|---|
| casus_belli `[有readme]` | 战争理由 | 00_default.txt |
| wargoals `[有readme]` | 战争目标（attacker/defender 块） | 00_default.txt |
| peace_treaties `[有readme]` | 和平条约 | 00_default.txt |
| diplomatic_costs | 外交行动成本 | 00_default.txt |
| join_war_rules `[有readme]` | 参战规则 | 00_default.txt |
| rival_criteria `[有readme]` | 宿敌标准（仅 AI） | 00_default.txt |
| subject_types `[有readme]` | 附庸类型 | 00_default.txt、conquistador.txt、appanage.txt |
| country_interactions `[有readme]` | 国家交互（diplo_chance 键全清单） | 00_default.txt、hre.txt… |
| character_interactions `[有readme]` | 角色交互 | 00_default.txt |
| scripted_relations `[有readme]` | 脚本化关系 | 00_default.txt |
| scripted_diplomatic_objectives `[有readme]` | 脚本化外交目标 | 00_default.txt |
| insults | 羞辱文本 | 00_default.txt |
| international_organizations `[有readme]` | 国际组织全套（含 land_ownership_rules/payments/special_statuses） | 00_default.txt |
| resolutions `[有readme]` | IO 决议 | 00_default.txt |
| parliament_agendas / parliament_issues / parliament_types `[有readme]` | 议会三件套 | 00_default.txt、01_country_specific_parliament_issues.txt |
| hegemons / formable_countries `[有readme]` | 霸权 / 可成立国家 | 00_default.txt |

## 内容系统（事件/任务/灾难）

| 类目 | 用途 | 入口文件 |
|---|---|---|
| on_action `[on_actions.info]` | 触发钩子（**info 文件是官方权威**） | _hardcoded.txt（引擎钩子全集）、country_monthly.txt、location_pulses.txt |
| scripted_effects `[有readme]` | 效果宏（$参数$） | 00_default.txt、country_effects.txt… |
| scripted_triggers `[有readme]` | 条件宏 | 00_default.txt、war_triggers.txt… |
| script_values `[_script_values.info]` | 数值公式（**info 文件是官方权威**） | define_values.txt、unit_values.txt… |
| scripted_lists | 脚本列表（random_in_list 用） | 00_default.txt |
| scripted_rules / scripted_modifiers / scripted_country_names / scripted_geography / scripted_guis / scriptable_hints | 各类脚本定义 | 00_default.txt |
| missions / mission_task_defs | 任务与任务项 | generic_*.txt |
| disasters `[有readme]` | 灾难 | 00_default.txt、decline_of_majapahit.txt |
| situations `[有readme]` | 局势 | 00_default.txt、little_ice_age.txt、black_death.txt |
| diseases `[有readme]` | 疾病 | bubonic_plague.txt |
| movements `[有readme]` | 思潮 | 00_default.txt |
| rebel_demands | 叛乱诉求 | 999_default_rebel_demands.txt、900_country_specific… |
| institution `[有readme]` | 制度 | 00_default.txt |
| historical_scores | 历史评分 | 00_default.txt |
| age `[无readme]` | 时代（age_1_traditions…） | 00_default.txt |
| advances `[有readme]` | 科技 | 0_age_of_traditions.txt…、2_army_unlocks.txt |
| societal_values | 社会价值轴 | 00_default.txt |
| customizable_localization | 可定制本地化（动态文本） | 00_default.txt |
| effect_localization / trigger_localization | 效果/触发自定义显示名 | 各 *_effects.txt、*_triggers.txt |

## 地图与地理

| 类目 | 用途 | 入口文件 |
|---|---|---|
| topography `[无readme]` | 地形（movement_cost/defender/weather 衰减/local_frontage_allowed） | 00_default.txt |
| vegetation `[无readme]` | 植被（woods/forest/jungle/desert…） | 00_default.txt |
| climates `[无readme]` | 气候带（winter 等级/has_precipitation/location_modifier） | 00_default.txt |
| road_types `[有readme]` | 道路 | 00_default.txt |
| area_preferences | 区域偏好（AI 扩张目标区） | 00_default.txt |
| location_ranks | 地点等级 | 00_default.txt |

## AI 与其他

| 类目 | 用途 | 入口文件 |
|---|---|---|
| ai_personalities | AI 人格 | 00_default.txt |
| ai_diplochance / ai_scripted_expansion_score / ai_scripted_expansion_target `[有readme]` | AI 外交概率/扩张评分 | 00_default.txt |
| generic_actions `[有readme]` / generic_action_ai_lists `[有readme]` | 通用行动与 AI 分组 | 00_default.txt |
| alert_descriptions | 警报定义（title/texture/priority） | 00_default.txt |
| attribute_columns | 列表视图列定义 | 13_combat.txt、05_location.txt |
| auto_modifiers `[有readme]` | 自动修正（country.txt 1555 行：country_base_values 等） | country.txt、location.txt |
| biases | 偏见（opinion 修正） | 01_opinion_scripted_diplomacy.txt… |
| artist_types / artist_work `[有readme]` | 艺术家 | 00_default.txt |
| music_player_tracks | 音乐 | 00_default.txt |
| tests `[有readme]` | 游戏自带测试用例 | 00_default.txt |
| tutorial_lessons / tutorial_lesson_chains | 教程 | 00_tutorial_lesson_diplomacy.txt |
| country_description_categories | 见政府组 | — |

## main_menu\common（13 类目）

achievements `[有readme]`（成就）、coat_of_arms（纹章）、flag_definitions（旗帜）、game_concepts（概念词条）、game_rules（游戏规则，`_game_rules.info` 权威）、modifier_icons（修正图标）、modifier_type_definitions（**修正键注册表**）、named_colors（颜色名）、scenarios（剧本）、scripted_lists / scripted_triggers / script_values（主菜单域脚本）、static_modifiers（静态修正）。

## loading_screen\common\defines

`00_defines.txt` + `graphic\00_graphics.txt` + `jomini\`（tooltips/adjacencies/fog_of_war/icons/mapeditor/rivers/roads）——见 `defines.md`。
