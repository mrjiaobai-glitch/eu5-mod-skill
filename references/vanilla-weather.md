# 原版解析：天气与气候系统（vanilla weather & climate）

版本基准：EU5 1.3.x（行号以实查为准，改版本后用 grep 重新定位）。全部结论来自游戏本体文件，路径相对 `<game>\`。

## 一、两层结构总览

```
[静态层] common\climates\00_default.txt   → 8 种气候带（winter 等级/降水/常驻修正）
[动态层] start_weather_system { ... }     → 引擎级天气实体（锋面/气旋/龙卷风）
              ▲ 生成
   common\on_action\location_pulses.txt   → weather_monthly_pulse 按月份生成
              ▼ 移动中
   common\topography\ + vegetation\       → weather_*_strength_change_percent 衰减弱度
              ▼ 到达地点
   on_action\_hardcoded.txt               → on_storm_reached_location（原版为空）
```

## 二、静态层：气候带（`common\climates\00_default.txt`，167 行）

8 种：`tropical` / `subtropical` / `oceanic` / `arid` / `cold_arid` / `mediterranean` / `continental` / `arctic`

| 字段 | 说明 | 原版取值示例 |
|---|---|---|
| `winter` | 冬季等级：none / mild / normal / severe | tropical/subtropical/arid/mediterranean = none；oceanic/cold_arid = mild；continental = normal；arctic = severe |
| `has_precipitation` | 是否有降水（视觉） | 仅 `arid`、`cold_arid` 为 `no` |
| `always_winter` | 需地形也有该标记才生效（视觉：山地永久积雪） | 仅 `arctic` |
| `location_modifier` | 常驻修正 | 如 tropical：人口容量 +50%、月度发展 −10%、寿命 −5、粮食衰减 0.002 |
| `colonial_migration_size_modifier` | 殖民迁移规模 | tropical −0.33、arctic −0.3、mediterranean +0.1 |
| `debug_color` / `color` | 调试与地图色 | — |

影响链条：气候的 `winter` 等级被小冰期局势等用作触发器（`winter_level = severe/normal/mild`，见 `events\situations\little_ice_age.txt`：severe 省份粮食 −15%、normal −10%、mild −5%）。

## 三、动态层：天气实体（引擎硬编码）

生成效果写在 `location_pulses.txt:50` 的注释里（官方格式）：

```
start_weather_system = { width = <pixels> length = <pixels> strength = [0..1]
                         speed = <pixels per day> type = <front/cyclone/tornado>
                         location = <起点> location = <航点> [location = <航点>...] }
```

| 类型 | 用途 | 原版典型参数 |
|---|---|---|
| `front`（锋面） | 季风、寒潮、沙尘暴 | width 800–1200、length 1–100、speed 30–50 |
| `cyclone`（气旋） | 飓风/台风 | width = length = 100、speed 30 |
| `tornado`（龙卷风） | 小范围风暴 | 见 `events\religion\hellenism_religion.txt:1571`（strength 0.5、speed 20） |

效果本地化已注册：`common\effect_localization\weather_effects.txt`（START_WEATHER_SYSTEM_EFFECT 等 4 键）。

## 四、生成逻辑：`weather_monthly_pulse`（`common\on_action\location_pulses.txt`，2324 行）

`root = location`，每月触发，按 `current_month` 分支：

| 分支 | 条件 | 内容 |
|---|---|---|
| 印度季风·雨季 | `current_month = 2` | 印度洋 → 喜马拉雅/云南，3 条平行锋面（宽 800/1000/1200）+ 3 条西支（经 diu/amet/delhi 入云南）；strength 0.5、speed 30 |
| 印度季风·旱季 | `current_month = 10` | 反向锋面（经 bairab/himalaya12/gaur 出洋）；strength 1、speed 30 |
| 北大西洋飓风季 | `current_month = 5/6/7` | `random_list`（40 权重空 + 十余条路径 10 权重）：塞内加尔/佛得角 → 佛罗里达/百慕大 → 里斯本/南特/布里斯托/苏格兰/法罗 |
| 极地涡旋 | `current_month = 1` | 15 条寒潮锋面（width 1000、length 1、strength 1、speed 50）：欧洲/俄罗斯/西伯利亚×4/白令/阿拉斯加/加拿大×3/格陵兰×2/冰岛/挪威 |
| 撒哈拉沙尘暴 | 见文件中段 | 多条北非路径 |
| 热带气旋 | 全年随机 | 东太平洋（约 1/40）、安达曼海（约每 2 年）、阿拉伯海（约每 4 年） |

## 五、强度衰减：defines + 地形

`loading_screen\common\defines\00_defines.txt` 的 `NWeather` 块（约 2564–2573 行）：

| 常量 | 值 | 含义 |
|---|---|---|
| `FRONT_DEGRADATION_DISTANCE_FOR_TOPOGRAPHY` | 125 | 锋面每移动 125 像素按下方地形修正一次强度 |
| `CYCLONE_/TORNADO_DEGRADATION_DISTANCE_FOR_TOPOGRAPHY` | 125 | 同上 |
| `*_DEGRADATION_PER_PIXEL_OF_LATITUDE` | 0 | 纬度衰减（原版禁用） |
| `PIXEL_COUNT_FOR_AVERAGE_LOCATION` | 1600 | 平均地点像素数 |
| `PERCENT_DIFFERENCE_BETWEEN_GOOD_AND_BAD_SIDE_OF_CYCLONE` | 50 | 气旋好侧/坏侧强度差 50% |

地形侧字段（`common\topography\00_default.txt`、`common\vegetation\00_default.txt`）：
`weather_front_strength_change_percent` / `weather_cyclone_strength_change_percent` / `weather_tornado_strength_change_percent`

⚠️ **原版全部为 0**，注释保留了设计值：flatland 注释 `-0.08`、hills `-0.4`、mountains `-2`（另有 `-4` 出现在某处）、森林 `-0.1`、海洋 `+0.01`。即原版天气不衰减——**这是 mod 可直接调用的空档**。

## 六、Gameplay 挂钩点

| 钩子 | 位置 | 说明 |
|---|---|---|
| `on_storm_reached_location` | `common\on_action\_hardcoded.txt:5493-5497` | `root = location`；`scope:weather_system`；**原版 effect 为空** —— 风暴的实际效果是引擎硬编码，脚本只能在此扩展 |
| `has_weather_system` 警报 | `common\alert_descriptions\00_default.txt:560`，橙色优先级 | 显示逻辑在 `gui\alertmanager.gui:2363-2380`（`AlertHasWeatherSystem.Next` 循环定位） |
| 天气地图模式 | `gfx\interface\icons\map_modes\weather.dds` | — |
| 天气 tooltip | `gui\shared\location_tooltips.gui:5984`（`WeatherSystem_tooltip`） | 用 `[WeatherSystem.GetNameWithNoTooltip]`、`[WeatherSystem.GetTooltip]` |

## 七、Mod 改造建议（可改 vs 硬编码）

| 想改什么 | 动哪里 |
|---|---|
| 生成时机/路径/强度 | 覆盖 `on_action\location_pulses.txt` 的 `weather_monthly_pulse`（或新增自己的 on_action 调 `start_weather_system`） |
| 天气衰减 | 填 `topography`/`vegetation` 的 `weather_*_strength_change_percent` + 调 `NWeather` defines |
| 风暴到达效果 | 填 `on_storm_reached_location` 的 effect（唯一脚本钩子） |
| 气候影响 | `common\climates\` 的 `location_modifier`（人口/粮食/寿命/补给） |
| 天气视觉/粒子 | `gfx\models\mapitems\weather\`（desert/sea/snow/volcano 各含 `_*_weather_entities.asset`） |

**硬编码不可改**：天气实体的移动、覆盖判定、衰减计算、好/坏侧伤害差异、风暴对单位的实际影响。

## 八、中文本地化检索键

`WEATHER_RAIN` / `WEATHER_TORNADO` / `WEATHER_CYCLONE` / `WEATHER_SANDSTORM`（调试事件选项，`events\debug\qa_debug.txt`）；`ALERT_WEATHER_SYSTEM_TITLE`（警报标题）。
