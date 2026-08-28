# 实测坑速查（pitfalls）

来源：`eu5-mod-review` 的实测记录（《刀锋与王座》2026-07/2026-08 实修，EU5 1.3.x）+ 本知识库翻阅游戏本体时的观察。**EU5 ≠ EU4**，以下都是真实踩过的坑。

## 一、EU4 语法混入（最常见）

| EU4 写法 | EU5 正确写法 |
|---|---|
| `ROOT` / `PREV` | `root` / `prev` |
| event_target / global_event_target | 无此概念；用 `save_scope_as = xxx` + `scope:xxx` |
| 本地化 `KEY:0` | 无 `:0` 后缀 |
| 事件选项 `weight = N` | `ai_chance = { base = N modifier = {...} }` 或 `ai_will_select = { <script math> }` |
| province_event | 无；用 `type = location_event` |
| `change_variable = { name = x value = N }` | `change_variable` 只有 add/subtract/multiply/divide/modulo；设值用 `set_variable` |

## 二、作用域坑（实测报错）

1. **save_scope_as 的具名 scope 不跨嵌套 effect 调用**（报 "Scoped object is not valid"）——effect 内复用须开头防御性重存：`scope:actor = { save_scope_as = xxx }`。
2. **random_subject 的 limit 用 has_variable 定位附庸不可靠**（原版 44 处用例无一配此组合）——用宗主保存的国家引用变量。
3. **every_owned_location 的 every_pop limit 勿再写 owner=root**（报 "Event target link 'owner' returned an invalid object"）——归属已由外层保证。
4. **mercenary scope 不能直接用 unit_location**（报 "Wrong scope: mercenary, expected unit"）——经 ordered_mercenary_sub_unit / every_mercenary_sub_unit 取子单位再取位置。
5. 事件选项里用 immediate 保存的 scope 做 title/desc 本地化是支持的，但**别在 title 里引用选项才有的 scope**。

## 三、变量坑

- 用前必初始化（未初始化刷 "Failed to fetch variable ... due to not being set"）。
- **初始化与使用侧同 scope**（跨 scope 是两份变量）。
- tooltip 显示：`[Root.GetVariable('x').GetValue|V0]`。

## 四、合并/结构坑

- **levies 特化单位必须放文件顶部**（第一个匹配生效）；INJECT 追加会排在 fallback 后永远轮不到——整体覆盖文件、新条目插 fallback 前。
- **REPLACE 必须逐字段保留原版内容**（如 pop_types 的 soldiers 若只改 max_strength，会丢 has_cap/estate 关联/literacy_impact）。
- **事件不能 REPLACE/INJECT**——复制修改会产生无害 "Duplicated event ID"。
- on_action 同名块是合并语义，直接写同名块追加即可，别加 REPLACE。
- **顺序敏感块**（country_name_construction 等"第一个匹配生效"）同样不能用 INJECT。

## 五、本地化坑

- **无 BOM 的 yml 整文件被忽略/乱码**；编辑工具保存会剥 BOM，记得补回。
- yml 键必须在语言头（`l_simp_chinese:`）下且缩进；顶格键会毁文件。
- 事件选项 name 写裸中文 → raw key；必须 `<ns>.<id>.a` 形式。
- 镜像：main_menu 与 in_game 两侧同键冲突时以加载顺序后者为准（eu5-mod-review 记为"加载 main_menu 侧为主"——**实测以版本为准，键尽量不重复定义**）。

## 六、AI 生态坑（观察者模式 160 年 0 触发教训）

- 带 `is_human = yes` 门槛的事件 AI 永不触发。
- ai_will_do / ai_chance 给 -100 起步 = AI 永不选。
- **灾难防重复**：can_start 的 `NOT{xxx_resolved=yes}` 标记**绝不能在 on_end remove**——否则灾难无限重复、永久奖励反复领取（实测单国 3 个永久修正）。

## 七、数量级/词条坑（实测）

- `num_forts` 等计数词条注意量级单位（游戏内数值 ≠ 直觉值，以原版用法为准）。
- `country_has_estate` 曾恒真（版本差异）——引用前先 grep 原版确认语义。
- **军队国（landless）碎国**问题：给无领土实体做机制时注意 `num_locations > 0` 类门槛。
- 社会价值轴 progress modifier 语义与直觉相反（正负号）——查原版 societal_values 用法。

## 八、本知识库翻阅时的观察（原版状态）

- **天气系统脚本侧留白**：topography 的 `weather_*_strength_change_percent` 全为 0（注释保留原设计值），`on_storm_reached_location` 为空——风暴的实际效果是引擎硬编码，mod 只能生成（start_weather_system）与挂钩子。
- **`defender` 地形骰子加成**：山脉 +2、丘陵/森林/林地/丛林/湿地 +1——防守方在恶劣地形有真实骰子优势，做地形平衡时注意。
- **is_garrison 全局唯一**：只有 1 个陆军类别可以有驻军标记（unit_categories readme）。
- **原版 mod 目录确认**：`Documents\Paradox Interactive\Europa Universalis V\mod\` 用 `.metadata\metadata.json` 注册（name/id/version/supported_game_version/short_description/tags），`supported_game_version` 如 "1.3.11"。
