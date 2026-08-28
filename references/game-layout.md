# 游戏本体结构地图（game-layout）

根：`<game>\`（本机 `F:\SteamLibrary\steamapps\common\Europa Universalis V\game\`）。四个顶层目录对应四种加载域：**in_game**（游戏内）、**loading_screen**（启动/加载）、**main_menu**（主菜单/全局静态数据）、**dlc**（DLC 包，本身就是 mod 结构模板）。

## in_game\（游戏内主域）

| 目录 | 内容 | 制作要点 |
|---|---|---|
| `common\` | 全部游戏机制数据，~160 个类目（见 `systems-map.md`） | mod 主要战场 |
| `events\` | 事件文件：顶层 26 个文件 + 15 个子目录（character / colonization / culture / debug / DHE / diplomacy / disaster / economy / estates / exploration / government / missionevents / religion / situations / ai_area_conqest_events） | 新事件放 `events\<前缀>_events.txt`；`readme.txt` 是权威格式 |
| `content_source\map_objects\` | 地图对象生成器（ambience_generators / generators / masks） | 地图美术层 |
| `fonts\` | 字体定义 | — |
| `gfx\` | 美术资源：animation_state_machines / city_materials / compound_nodes / graphical_culture_types / images / map / models / scenes / terrain2 / music_player_gui | mod 美术放同名子目录 |
| `gui\` | ~180 个 `.gui` 文件 + 子目录 filters / panels / shared / map_objects | UI；mod 一般不改 |
| `localization\` | **只有** `jomini\script_system\trigger_system_l_<lang>.yml`（引擎触发词本地化，11 语言） | mod 语言文件**不**放这里（见 `localization.md`） |
| `map_data\` | default.map / definitions.txt / adjacencies.csv / ports.csv / nodes.dat / location_templates.txt / locations.png / rivers.png | 地图底层，mod 极少动 |
| `setup\countries\` | 国家定义：按地区分文件（anatolia.txt、british_isles.txt …）+ `00_readme.info`（TAG 格式权威） | 新国家/改国家在这 |

## loading_screen\（加载域）

| 路径 | 内容 |
|---|---|
| `common\defines\00_defines.txt` | **全部引擎常量**（2609 行）：NGame/NCombat/NPop/NUnit/NWeather/NCharacter/NLocation 等 ~30 个 N 块（索引见 `defines.md`） |
| `common\defines\graphic\00_graphics.txt` | 图形常量 |
| `common\defines\jomini\` | 引擎级：00_tooltips / adjacencies / fog_of_war / icons / mapeditor / rivers / roads |
| `localization\<lang>\` | 语言文件（languages.yml、load_tips_l_<lang>.yml） |
| `localization\jomini\` | savegame_gui_l_<lang>.yml、cw_ugc_dlc_l_<lang>.yml |

## main_menu\（主菜单域，含全局静态数据）

`common\` 13 个类目（mod 常用前 5 个）：

| 类目 | 用途 |
|---|---|
| `static_modifiers\` | **静态修正定义**：country.txt（12937 行）、character.txt、difficulty.txt、capital_in_topography.txt 等；格式 = `键 = { game_data = { category = <country/location/...> } <modifier> = 值 }` |
| `modifier_type_definitions\00_modifier_types.txt` | **修正键注册表**（17616 行）：新增修正键必须在此声明 category/percent/boolean/format/min/max/cap_zero_to_one/scale_with_pop/ai/bias_type/should_show_in_modifiers_tab（文件头注释即权威） |
| `game_rules\00_game_rules.txt` + `_game_rules.info` | 游戏规则：default 设置、apply_modifier（player/ai/all 分类）、**defines 覆盖**（在规则里改 define）、flag（blocks_achievements 等） |
| `game_concepts\00_game_concepts.txt` | 概念词条（百科链接文本） |
| `achievements\` | 成就：standard_achievements.txt + 各 DLC；须注册进 achievement_groups.txt |
| `coat_of_arms\` | 纹章（coa_templates / 预置国家纹章） |
| `flag_definitions\00_flag_definitions.txt` | 旗帜定义 |
| `modifier_icons\` | 修正图标映射 |
| `named_colors\` | 颜色命名 |
| `scenarios\00_scenarios.txt` | 剧本 |
| `scripted_lists\` / `scripted_triggers\` / `script_values\` | 主菜单域脚本（加载较早，可被游戏内引用） |

`localization\<lang>\`：**语言文件主目录**（english 114 个 yml，按主题命名 `<主题>_l_english.yml`，如 actions_l_english.yml、advances_l_english.yml、buildings_l_english.yml）。

## dlc\（DLC = mod 结构模板）

- `D000_shared\`：main_menu 共享（common\dlc、gfx\interface、localization\dlc\<lang>）——"共享包"最小模板
- `D008_fate_of_the_phoenix\`：完整模板：in_game / loading_screen / main_menu 三分区 + 根文件 `D008_fate_of_the_phoenix.dlc.json`（dlc 元数据，形似 metadata.json）+ thumbnail.dds + checksum_manifest.txt
- `D015_ancient_monuments_pack\`、`D017_sacred_sites_pack\`：内容包

## 关键数量事实（实查）

- `in_game\common\` 共 **73 个 readme.txt**（72 个类目 + production_methods\__readme.txt）+ `setup\countries\00_readme.info`
- `main_menu\localization\english\` **114 个 yml**（每语言同数；russian 131、german/spanish 118 略多）
- `main_menu\common\static_modifiers\country.txt` 12937 行、`00_modifier_types.txt` 17616 行——改原版修正前先 grep 这些文件
- 引擎触发词条在 `in_game\localization\jomini\script_system\trigger_system_l_<lang>.yml`（11 语言，各 1 文件）
