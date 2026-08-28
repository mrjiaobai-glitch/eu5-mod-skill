# Defines 体系（defines）

位置：`loading_screen\common\defines\`（三处）：
- `00_defines.txt`（2609 行）——全部引擎常量
- `graphic\00_graphics.txt`——图形常量
- `jomini\`——引擎层：00_tooltips.txt / adjacencies.txt / fog_of_war.txt / icons.txt / mapeditor.txt / rivers.txt / roads.txt

## 00_defines.txt 的 N 块索引（实查）

| 块 | 行区间（约） | 内容 |
|---|---|---|
| NGame | 1–33 | 时间（START_DATE "1337.4.1" / END_DATE "1836.12.31"）、HOUR_TICK=2、游戏速度表 |
| NCityAudio / NAlertAudio / NJominiMap / NGUI / NText | 35–116 | 音频/地图/GUI/文本 |
| NCountry | 117–298 | 国家级（稳定度、威望、厌战等） |
| NMercenary | 299–324 | 雇佣兵（MERCENARY_PRISONER_MAX_MORALE=0.6） |
| **NUnit** | 325–399 | 单位：REGIMENT_SIZE=1000、LAND_MORALE=3.0、NAVAL_MORALE=3.0、MONTHLY_REINFORCE=0.25、MONTHLY_REPAIR=0.1、ATTRITION_LACK_OF_FOOD=5、LEVY_MAINTENANCE_FACTOR=0.01、ARMY_MOVEMENT_SPEED=0.13、NAVY_MOVEMENT_SPEED=0.5 |
| **NCombat** | 402–513 | 战斗：COMBAT_DICE_SIDE=10、COMBAT_BASE=5、COMBAT_MAX=15、COMBAT_DAMAGE_MULT=0.01、HOURS_PER_PHASE=5、MINIMUM_COMBAT_DURATION=24、MINIMUM_NAVAL_COMBAT_DURATION=72、STRAIT_CROSSING_DICE=-2、RIVER_CROSSING_DICE=-1、SEA_LANDING_DICE=-1、MAX_FRONTAGE_OVERSTACKING=1.25、LAND_LEVY_COMBAT_IMPACT=0.75、INITIATIVE_*、ASSAULT_*、SIEGE_*、DAYS_PER_SIEGE_PHASE=30、MAX_BREACH=3、TRADITION_GAIN_LAND=10 |
| NAI | 515–1446 | AI：BATTLE_WIN_CHANCE_*、AI_REINFORCE_BATTLE_*、AI_ATTACH_FRONTAGE_* 等 |
| NCulture / NCharacter / NPortrait | 1447–1590 | 文化；角色（MAX_ABILITY_VALUE=100、LEADER_COMBAT_TRAIT_GAIN_CHANCE=0.5、ASSIMILATION_* 同化修正表 1487–1506） |
| **NPop** | 1591–1637 | POP：MIN_REBEL_POP_SIZE_THRESHOLD=5、DISPLAY_SIZE=1000（显示=千）、BASE_SATISFACTION=0.2、主文化+0.25/接纳+0.10/容忍+0.05、pop_join_rebel_threshold 相关 REBEL_JOIN_MIN_THRESHOLD=0.05、POP_MINORITY_SIMILAR_THRESHOLD=4/8、YEARLY_POP_SATISFACTION_FROM_EVENTS_DECAY=0.01 |
| NEstate | 1639–1688 | 阶层 |
| **NLocation** | 1689–1752 | 地点：SUPPLY_LIMIT_OWNER=0.25、FORT_GARRISON_UPKEEP=2、MIN_FRONTAGE_AFTER_TERRAIN=2、NORMAL_LOCATION_SIZE=25、MONTHLY_CONTROL_DECAY=0.01 |
| NMarket / NEconomy | 1754–1991 | 市场/经济：MONTHLY_PRICE_CHANGE=0.05、GROWTH_FROM_FOOD_MULTIPLIER_MAX=10、LOCATION_DEPOPULATION_ALERT_MONTHS=60 |
| NDiplomacy / NWar | 1992–2480 | 外交/战争：DEFAULT_WARGOAL_BATTLESCORE_BONUS=3、WARSCORE_MAX_FROM_BATTLES=50、BATTLE_RESULT_SCALE=25、PARTICIPATION_SCORE_BATTLE=0.03 |
| NWorkOfArt / NDynasty / NColony | 2481–2551 | 艺术品/王朝/殖民 |
| NInternationalOrganization / NImperialCircle | 2552–2562 | 国际组织/帝国圈 |
| **NWeather** | 2564–2573 | 天气：*_DEGRADATION_DISTANCE_FOR_TOPOGRAPHY=125、PERCENT_DIFFERENCE_BETWEEN_GOOD_AND_BAD_SIDE_OF_CYCLONE=50 |
| NReligion / NSpreadable / NDisease | 2575–2609 | 宗教/传播/疾病 |

（行号基于 1.3.x 版本，改版本后以 grep 定位为准。）

## 修改方式

1. **直接覆盖**：mod 里建 `loading_screen\common\defines\00_defines.txt`，只写要改的块（同名块合并，键级覆盖）。
2. **游戏规则覆盖**（推荐，玩家可选）：`main_menu\common\game_rules\00_game_rules.txt` 的 setting 里写 `defines = { NCombat = { COMBAT_BASE = 6 } }`（格式见 `_game_rules.info`），并可配 flag 阻止成就。
3. **图形常量**：`graphic\00_graphics.txt`。

## 关键事实

- `HOUR_TICK = 2`：游戏按 2 小时 tick；战斗阶段 `HOURS_PER_PHASE = 5`。
- `START_DATE = "1337.4.1"`、`END_DATE = "1836.12.31"`：事件时间窗（dynamic_historical_event 的 from/to）应落在此区间。
- 修改 defines 会**改变存档兼容性与多人哈希**，平衡性改动优先用修正（static_modifiers/auto_modifiers）而非 defines。
