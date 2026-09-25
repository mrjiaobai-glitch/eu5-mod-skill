# 原版解析：战斗系统（vanilla combat）

版本基准：EU5 1.3.x。核心文件：`loading_screen\common\defines\00_defines.txt`（`NUnit` 325–399、`NCombat` 402–513）、`common\unit_categories\`、`common\unit_types\`、`common\topography\`、`common\vegetation\`、`common\wargoals\`。

## 一、部队组织

- 团（regiment）= `REGIMENT_SIZE = 1000` 人；每团有 **strength（兵力）** 与 **morale（士气）** 两条
- 基础士气 `LAND_MORALE = 3.0` / `NAVAL_MORALE = 3.0`
- 补充 `MONTHLY_REINFORCE = 0.25`（陆）/ `MONTHLY_REPAIR = 0.1`（海）
- 移动 `ARMY_MOVEMENT_SPEED = 0.13` / `NAVY_MOVEMENT_SPEED = 0.5`；规模影响移动（`SIZE_IMPACT_ON_MOVEMENT_SCALE = -0.02`，封顶 `-0.5`）
- 缺粮损耗 `ATTRITION_LACK_OF_FOOD = 5`、海上损耗 `ATTRITION_DAYS_AT_SEA = 0.02`、冻结损耗 `FROZEN_ATTRITION = 0.5`

## 二、单位体系

### 陆军 6 类（`common\unit_categories\`）

| 类别 | frontage | initiative | combat_speed | flanking | 特性 |
|---|---|---|---|---|---|
| 轻步兵 `00_army_light_infantry` | 1 | 5 | 3 | 1.1 | assault=yes；morale_damage_taken +0.10 |
| 重步兵 `01_army_heavy_infantry` | — | — | — | — | 近战主力 |
| 轻骑兵 `02_army_light_cavalry` | — | — | — | — | 侧翼 |
| 重骑兵 `03_army_heavy_cavalry` | — | — | — | — | 正面突破，革命时代后过时 |
| 炮兵 `04_army_artillery` | 1 | **1** | **1** | 1.0 | bombard=yes；`damage_taken = 1.25`、`attrition_loss = 0.5`、`food_consumption 0.66` |
| 辅助 `05_army_auxiliary` | — | — | — | — | 运粮 |

海军 4 类：galley / light_ship / heavy_ship / transport（属性 `cannons`、`hull_size`、`crew_size`、`maritime_presence`、`blockade_capacity`、`transport_capacity`）。

### 兵种模板与升级（`common\unit_types\`）

- `00_age_templates_land.txt`：6 个时代的模板，**数值随时代翻倍**
  | 时代 | 轻/重步兵 max_strength / combat_power | 炮兵 combat_power / bombard_efficiency / artillery_barrage |
  |---|---|---|
  | 1 traditions | 0.5 / 1 | 2 / 0.1 / 1 |
  | 2 renaissance | 1.0 / 1 | 3 / 0.125 / 3 |
  | 3 discovery | 1.5 / 1 | 4 / 0.15 / 4 |
  | 4 reformation | 2.0 / 1.5 | 5 / 0.20 / 5 |
  | 5 absolutism | 2.5 / 2.25 | 5.5 / 0.25 / 7 |
  | 6 revolutions | 3.0 / 3 | 7.5 / 0.30 / 9 |
- 海军模板 `00_age_templates_navy.txt`：重型船 hull 15→60、cannons 30→120；运输船 transport_capacity 0.25→1.5、food_storage 30→960
- 升级链 `upgrades_to`（如 `n_carrack → n_galleon → n_twodecker → n_threedecker`）；`copy_from` 继承
- 特殊兵种：`3_janissaries.txt`（每代 `strength_damage_taken = -0.10`、`morale_damage_taken = -0.10`，受 `janissary_unit_limit` 限制）、骑士 `0_knights.txt`、征服者、大象、卡瓦、齐兹尔巴什
- 征召兵 `levy = yes` → 战力 ×`LAND_LEVY_COMBAT_IMPACT = 0.75`
- 雇佣兵池 `mercenaries_per_location = { pop_type = X multiply = N }`（如 laborers 0.1、nobles 0.3）
- 全修饰符清单见 `unit_types\readme.txt`（frontage/initiative/combat_speed/flanking_ability/secure_flanks_defense/combat_power/max_strength/damage_taken…）

## 三、战斗常数（`NCombat`）

| 常量 | 值 | 说明 |
|---|---|---|
| `COMBAT_DICE_SIDE` | 10 | 10 面骰 |
| `COMBAT_BASE` / `COMBAT_MAX` | 5 / 15 | 骰值基准与上限 |
| `COMBAT_DAMAGE_MULT` | 0.01 | 伤害倍率 |
| `HOURS_PER_PHASE` | 5 | 每阶段 5 小时 |
| `MINIMUM_COMBAT_DURATION` | 24 | 陆战最少 24 小时 |
| `MINIMUM_NAVAL_COMBAT_DURATION` | 72 | 海战最少 72 小时 |
| `STRAIT_CROSSING_DICE` | −2 | 海峡 |
| `RIVER_CROSSING_DICE` | −1 | 渡河 |
| `SEA_LANDING_DICE` | −1 | 登陆 |
| `MAX_FRONTAGE_OVERSTACKING` | 1.25 | 侧翼可超编 25% |
| `MIN_FRONTAGE_AFTER_TERRAIN` | 2 | 地形削减后最低正面宽度（在 `NLocation`） |
| `MORALE_COLLAPSE_THRESHOLD` | 0.05 | 士气崩溃 |
| `COMBAT_HOURLY_MORALE_TICK` | 0.01 | 每小时士气流失 |
| `INITIATIVE_BASE_CHANCE / _EACH / _HOURS / _MAX` | 0.1 / 0.02 / 0.01 / 0.1 | 主动性决定接敌概率 |
| `COMBAT_SPEED_SCALE` | 0.05 | 战斗速度影响攻击频率与撤退 |
| `RETREAT_STRENGTH_DAMAGE` | 0.1 | 撤退损失 10% 兵力 |
| `LAND_EXPERIENCE_DAMAGE_REDUCTION` | 0.5 | 经验减伤上限 50% |
| `EXPERIENCE_GAIN` | 30 | 战斗经验 |
| `TRADITION_GAIN_LAND / _NAVAL` | 10 / 20 | 传统 |
| `COMBAT_IMPRISONED_UNIT_DEATH_RATE` | 0.4 | 俘虏死亡率 |
| `LAND_WAR_EXHAUSTION_FROM_LOSSES` | 1（海军 ×1.5），`MAX_WAR_EXHAUSTION_FROM_BATTLE` 5.0 | 厌战 |
| `PRESTIGE_FROM_LAND/_NAVAL`、`PRESTIGE_VS_RIVAL` | 0.5 / 0.5 / 1.5 | 威望（打宿敌 ×1.5） |
| 海战专属 | `NAVAL_MORALE_DAMAGE_MODIFIER 0.2`、`NAVAL_LOW_MORALE_THRESHOLD 1.5`、`NAVAL_COMBAT_SHIP_STR_SINK_THRESHOLD 0.1`、`NAVAL_RETREAT_CHANCE 10` | 海战士气伤害仅 20% |

## 四、地形修正

**骰子加成**（`topography`/`vegetation` 的 `defender` 字段）：

| 地形 | defender | frontage 惩罚 | 备注 |
|---|---|---|---|
| 山脉 mountains | **+2** | `local_frontage_allowed = -4` | `blocked_in_winter = yes` |
| 丘陵 hills | +1 | −3 | — |
| 森林 forest | +1 | −3 | — |
| 林地 woods | +1 | −2 | — |
| 丛林 jungle | +1 | −4 | — |

（另有湿地植被 +1；`local_frontage_allowed` 也出现在 location_modifier 中影响可用宽度。）

**兵种地形修正**（unit 的 `combat = {}` / `impact = {}`）：骑兵模板自带 `jungle/wetlands/mountains = -0.10`（战斗伤害 −10%）、`impact` 同地形 +0.10（移动更慢）；还支持 `river`、`coastal`、`inland`、`climate` 键。

## 五、围城（`NCombat` 后半段）

| 常量 | 值 |
|---|---|
| `DAYS_PER_SIEGE_PHASE` | 30（无堡垒 `DAYS_PER_SIEGE_PHASE_WITHOUT_FORT = 15`，下限 `MIN_DAYS_PER_SIEGE_PHASE = 7`） |
| `SIEGE_WIN` | 20（阶段骰达 20 破城） |
| `MAX_BREACH` | 3；`BREACH_REPAIR_PER_DAY = 0.01` |
| `SIEGE_MEMORY` | 11 |
| 短缺惩罚 | 补给 −0.02 / 粮食 −0.03 / **水源 −0.05** / 守军逃亡 −0.1 / 破口 −0.05 |
| `SIEGE_DISEASE_IMPACT` | 0.05 |
| 轰炸 | `BOMBARD_BASE_CHANCE = 0.2`、`BOMBARD_HOURS = 5` |
| 强攻 | 攻方 `ASSAULT_ATTACKER_LOSS = 2.5`、士气 −3.0；守方 0.03 / 士气 −0.3；`ASSAULT_DICE_MODIFIER = 5`、`ASSAULT_WIDTH_LIMIT = 1` |
| 占领获得 | `GARRISON_AFTER_OCCUPATION = 0.010`、`FOOD_PERCENTAGE_LOST_AT_OCCUPATION = 0.9` |
| 炮兵门槛 | `SIEGE_REGIMENTS_FOR_ARTILLERY = 3`；`FORT_GARRISON_UPKEEP = 2`（NLocation） |

驻军强度脚本：`common\script_values\garrison.txt`（根为 location；`garrison_strength`、`combat_side_strength`、`besieger_strength`，输出守/攻比值用于出击判定）。

## 六、战争目标（`common\wargoals\00_default.txt`）

`take_capital`（约 105–116 行）：`type = take_capital`；attacker `conquer_cost = 0.5`、`subjugate_cost = 0.5`；`ticking_war_score = 0.5`（每月 +0.5 战争分数）。变体：`take_capital_sound_toll`（成本 1.5）、`take_capital_tributary`、`take_capital_subjugation`、`take_capital_imperial`。

## 七、脚本钩子与 AI 常量

**on_action（`common\on_action\_hardcoded.txt`）**：
- `on_battle_won` / `on_battle_lost`（约 2790 / 2916 行）、`on_great_battle_won` / `on_great_battle_lost`（约 2609 / 2747）
- scope 约定（文件注释）：`root = actor`、`scope:actor` = 胜方单位、`scope:target` = 败方单位；浮点 scope：`scope:killed_land_units`、`scope:killed_navy_units`、`scope:lost_land_units`、`scope:lost_navy_units`、`scope:war_score`
- `in_battle`（488–491）：`root = character`（最高指挥官），每 tick 触发
- `on_siege_won`（278）/ `on_siege_lost`（351）
- 原版用例：科索沃战役变量、帖木儿击杀计数、特殊单位经验（`grant_special_unit_experience`）

**AI 战斗常量（`NAI`）**：`BATTLE_WIN_CHANCE_GENERAL_MIL_FACTOR = 0.25`（100 军事 ≈ +25% 等效兵力）、`INITIATIVE_COMBAT_STRENGTH_FACTOR = 0.025`、`AI_FLANKING_COMBAT_STRENGTH_FACTOR = 0.3`、`AI_RECOVER_MORALE_THRESHOLD = 66`、`AI_RETREAT_DICE_MORALE_THRESHOLD = 0.45`、`AI_RETREAT_FLANK_MORALE_THRESHOLD = 0.40`、`AI_REINFORCE_BATTLE_DISTANCE_LIMIT = 3`。

**战争分数**（`NDiplomacy`）：`BATTLE_RESULT_SCALE = 25`、`BATTLE_RESULT_CAP = 25`、`WARSCORE_MAX_FROM_BATTLES = 50`、`DEFAULT_WARGOAL_BATTLESCORE_BONUS = 3`。

## 八、Mod 改造建议

| 想改什么 | 动哪里 |
|---|---|
| 兵种数值 | `common\unit_types\`（copy_from 时代模板改）或 `unit_categories\` |
| 新兵种 | 新建 unit_types 文件 + `category` 指向现有类别（`copy_from` 模板） |
| 地形战斗修正 | `topography`/`vegetation` 的 `defender`、`local_frontage_allowed`、`combat = {}` |
| 战斗全局数值 | `NCombat` / `NUnit` defines（或 game_rules 覆盖） |
| 战斗事件 | `on_battle_won/lost`、`in_battle`（写同名块追加） |
| 战争目标 | `common\wargoals\`（注意 `type` 是引擎枚举，新增须用现有 type） |
| 驻军/围城脚本 | `common\script_values\garrison.txt`、`NCombat` 围城段 |

**硬编码**：骰子结算、接敌判定（主动性）、伤害公式主体、侧翼包抄逻辑、撤退判定。

## 九、中文检索键

`morale`（士气）、`frontage`（正面宽度）、`initiative`（主动性）、`combat_speed`（战斗速度）；`wargoals` 的中文名在各 `*_l_simp_chinese.yml` 的 `war_goal_*` 键。
