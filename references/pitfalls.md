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
6. **on_action 的 on_actions 列表引用带 trigger 的子块时，子块 trigger 可能不被评估**（2026-09 实测：landless_passage_pulse 挂在 monthly_country_pulse 的 on_actions，其 trigger 要求"军队型+流亡标记"，结果全部原版军队国（察合台等，无任何标记）都执行了 effect、收到专属事件）。原版同文件子块（on_papal_opinion_added）trigger 正常 → 疑似"跨文件合并块引用的子块"才触发此问题。**对策：关键身份/条件门槛内聚到事件自身 trigger + 调用处 effect 内 if（双保险），不要只依赖 on_action 子块 trigger**。事件自身 trigger 实测可靠（加门槛后不再误弹）。

## 三、变量坑

- 用前必初始化（未初始化刷 "Failed to fetch variable ... due to not being set"）。
- **初始化与使用侧同 scope**（跨 scope 是两份变量）。
- tooltip 显示：`[Root.GetVariable('x').GetValue|V0]`。
- **`exists = var:X` 只宜用于"国家引用变量"**（变量里存 scope 时检查引用有效性）；**数值变量的存在性守卫要用 `has_variable = X`**——trigger 上下文用 `exists = var:<数值变量>` 实测守卫不生效（初始化被跳过 → 后续读取连锁报 "Failed to fetch variable" + "Event target link 'var' returned an unset scope"，error.log 单会话 670 次）。
- 索饷/计数类 tooltip 变量（GetVariable 渲染）在事件 **immediate 里做幂等兜底初始化**（`if NOT has_variable → set 0`），否则老档/无历史时悬停选项报 "Data error in loc string"。

## 四、合并/结构坑

- **levies 特化单位必须放文件顶部**（第一个匹配生效）；INJECT 追加会排在 fallback 后永远轮不到——整体覆盖文件、新条目插 fallback 前。
- **REPLACE 必须逐字段保留原版内容**（如 pop_types 的 soldiers 若只改 max_strength，会丢 has_cap/estate 关联/literacy_impact）。
- **事件不能 REPLACE/INJECT**——复制修改会产生无害 "Duplicated event ID"。
- on_action 同名块是合并语义，直接写同名块追加即可，别加 REPLACE。
- **顺序敏感块**（country_name_construction 等"第一个匹配生效"）同样不能用 INJECT。

## 五、本地化坑

- **无 BOM 的 yml 整文件被忽略/乱码**；编辑工具保存会剥 BOM，记得补回。**DSH 的 `write` 与 `edit` 对 `.txt`／`.yml`／`.json`／`.md` 一律剥 BOM**（2026-09 实测累计 7 次：`my_big_house.txt`、`strengthen.txt`、`metadata.json`、`设计说明.md`、两个法律文件、yml 补丁）——而 mod 的惯例是**全部带 BOM**，**每次编辑后必须查 BOM**（读前 3 字节 EF BB BF）。**这不是"偶尔会忘"，是"每次都会发生"**——不要靠记性，靠脚本复查。
- **注意区分**：技能库自身的 `.md`（`~\.dsh\skills\`）**不带 BOM**（已实测 5 个文件全无）。BOM 要求只针对 mod 目录下的文件。
- yml 键必须在语言头（`l_simp_chinese:`）下且缩进；顶格键会毁文件。
- 事件选项 name 写裸中文 → raw key；必须 `<ns>.<id>.a` 形式。
- 镜像：main_menu 与 in_game 两侧同键冲突时以加载顺序后者为准（eu5-mod-review 记为"加载 main_menu 侧为主"——**实测以版本为准，键尽量不重复定义**）。

## 六、AI 生态坑（观察者模式 160 年 0 触发教训）

- 带 `is_human = yes` 门槛的事件 AI 永不触发。
- ai_will_do / ai_chance 给 -100 起步 = AI 永不选。
- **灾难防重复**：can_start 的 `NOT{xxx_resolved=yes}` 标记**绝不能在 on_end remove**——否则灾难无限重复、永久奖励反复领取（实测单国 3 个永久修正）。

## 七、数量级/词条坑（实测）

- `num_forts` 等计数词条注意量级单位（游戏内数值 ≠ 直觉值，以原版用法为准）。
- `country_has_estate` **恒真**（不是版本差异）：EU5 为每个国家创建所有阶层对象，该触发器只检查"对象是否存在" = 永远 yes。判"阶层实质存在"用 `"estate_power(estate_type:xxx)" > 0`——详见 `blades-and-thrones-2026-08.md` 第 1 节。
- **`is_subject_type = <mod 自定义类型>` 疑似恒真**（2026-09 实测，与 country_has_estate 同族）：无 overlord、无任何附庸关系的独立原版军队国通过了 `is_subject_type = mercenary_company` 检查（存档实证无 subject 字段、事件自身 trigger 也拦不住）；而原版类型（is_subject_type = colonial_nation 等）工作正常。**推论：对 mod 新增 subject type 的 is_subject_type 门槛可能全部失效**（行动/改革 potential、AI 排除列表等 100+ 处受影响）——规避法：创建时给国家打变量（set_variable = yes），门槛用变量检查。验证技巧："独立国对照法"——把怀疑恒真的触发器放到一个普通国家身上看是否误通过。
- 引擎按名识别 static modifier 是常态：`capital_in_*`（首都地形）、`ruler_/general_/admiral_/explorer_*`（角色属性）、`difficulty_*`/`low_aggression`（难度/AI）、`<estate>_tax_impact`（阶层拼接）、`<side>_progress_cabinet_efficiency`（价值观轴）等**零脚本引用但生效**——"零引用=死代码"审计必须豁免引擎概念名与 INJECT: 指向原版对象的合并文件（目标名只在原版出现）。
- **军队国（landless）碎国**问题：给无领土实体做机制时注意 `num_locations > 0` 类门槛。
- 社会价值轴 progress modifier 语义与直觉相反（正负号）——查原版 societal_values 用法。
- 原版事件也可能是报错源：`flavor_kor.32` 的 trigger `unit.unit_location.owner ?= ROOT` 在将军无部队时刷 "Invalid unit found in scope"（1.3.11）——玩高丽必现，与本 mod 无关，审 error.log 先看 "Script location" 归属。

## 八、本知识库翻阅时的观察（原版状态）

- **天气系统脚本侧留白**：topography 的 `weather_*_strength_change_percent` 全为 0（注释保留原设计值），`on_storm_reached_location` 为空——风暴的实际效果是引擎硬编码，mod 只能生成（start_weather_system）与挂钩子。
- **`defender` 地形骰子加成**：山脉 +2、丘陵/森林/林地/丛林/湿地 +1——防守方在恶劣地形有真实骰子优势，做地形平衡时注意。
- **is_garrison 全局唯一**：只有 1 个陆军类别可以有驻军标记（unit_categories readme）。
- **原版 mod 目录确认**：`Documents\Paradox Interactive\Europa Universalis V\mod\` 用 `.metadata\metadata.json` 注册（name/id/version/supported_game_version/short_description/tags），`supported_game_version` 如 "1.3.11"。

## 九、死代码审计与存档分析要点（2026-09 实测补充）

**死代码审计豁免清单**（"定义零引用" ≠ 死，以下类别豁免）：
- auto_modifier / disaster / regency / subject_type / societal_value：引擎按 potential_trigger/can_start 等驱动，定义即生效；
- static modifier 引擎概念名（见第七节）与 `<estate>_`/`<side>_` 拼接约定；
- `INJECT:/REPLACE:` 指向**原版对象**的合并文件（目标名只在原版出现——在 mod 内部找不到引用是正常的）；
- 纯玩家手动授予的对象（privilege 等，有 loc 即正常）；generic_action 的引用在 .gui；
- `*_display` 变量：被 .yml 的 `GetVariable('...')` 读取（脚本侧扫描要覆盖 .yml）。

**存档分析要点**（明文 .eu5，数百 MB）：
- 变量存 `flag=<名> data={ type=... identity=... }`；变量名可全文 IndexOf；
- **附庸关系不以 `subject_type=<文本>` 存储**——别用文本搜判断附庸/独立（overlord 也非文本字段）；
- 存档 metadata **不含启用 mod 列表**——验证"游戏跑的是哪个版本"用 error.log 的 Script location 行号对照（本地删过行 vs 旧版行号差）；
- 380MB 档禁止正则整档扫描（超时）；用 IndexOf 循环 + 命中点前 ~30 万字符回溯 `country_name="TAG"` 定位国家；
- 判断"国家是否被某机制转化过"看机制副作用（ai_personality 变化、country_type 变化）与变量残留——注意部分转化路径会 remove 标记，残留为 0 不能反证没转化过，要看不可逆副作用。

## 十、修正词条与数值校准（2026-09 实测）

### 凭印象必写错的词条

| 词条 | 陷阱 |
|---|---|
| `marriage_desirability` | **`category = character`** —— 不能用在 `country_modifier` 里（做"联姻"类政策时最容易踩） |
| `subject_not_obligated_to_join_war` | **`boolean = yes`** —— 不能给数值 |
| `casus_belli_creation_speed` | 本体**几乎无用例**（仅一个可疑的 `4.0`），量级无法校准 → 避开 |
| `global_manpower` | **不存在**；国级是 `global_manpower_modifier`（`00_modifier_types.txt:4069`），地方级才是 `local_manpower` |
| `global_army_tradition` | **不存在**；国级陆军传统是 `monthly_army_tradition` |
| `stability_cost_modifier`／`global_unrest`／`global_trade_power`／`max_absolutism`／`global_monthly_devotion` | 均**不存在** |

### 符号语义反直觉（填数值最容易写反）

`antagonism_received_modifier` 与 `diplomatic_spending_cost` 都是 **`color=bad`** —— **正值是坏事、负值是好事**。

### 量级陷阱：`local_*` 与 `global_*` 不同档

曾拿 `town_rights` 的 `local_monthly_literacy = 0.05` 去校准国级 `global_monthly_literacy`，写出的值比本体众数高 5 倍。**国家级的量级必须用国家级用法校准**，`local_*` 的档位不能外推到 `global_*`。

### 数值校准法（众数法）

从 `in_game\common\` 抽取该修正的**取值分布** → 取**众数** → 吸附到本体**实际出现过的精确值**（不要外推、不要取整到好看的数字）。实测锚点：

| 修正 | 众数／依据 |
|---|---|
| `legislative_efficiency` | 0.1 |
| `stability_decay` | −0.00025（19×），最低只到 −0.005 |
| `global_monthly_literacy` | 0.01（28×） |
| `global_population_growth` | 0.0001（42×） |
| `monthly_legitimacy` | 0.05 |
| `global_<estate>_estate_power` | ±0.1 |
| `embrace_institution_cost_modifier` | −0.10 |
| `subject_income_modifier` | 0.025（8×） |
| `antagonism_received_modifier` | −0.1（21×）；**正值全本体仅 +0.1 一例** |
| `monthly_prestige` | 0.1（89×） |
| `global_distance_from_capital_speed_propagation` | 0.1（44×），1637 档的 `unitary_administration` 用 0.05 |
| `global_manpower_modifier` | 众数 0.1，但 0.15 有 10 处用例 |
| `global_levy_recruitment_speed_modifier` | **本体最低档就是 0.1**（写 0.05 即超限） |

### 脚本值实测（`main_menu\common\script_values\default_values.txt`）

| 脚本值 | 值 |
|---|---|
| `societal_value_monthly_move` | 0.1（本体最常用漂移档） |
| `societal_value_minor_monthly_move` | 更小档（本体用于 minor 漂移） |
| `small_permanent_target_satisfaction` | 0.025 |
| `medium_permanent_target_satisfaction_penalty` | −0.05 |
| `diplomatic_reputation_mild_bonus` | 2 |

### 存在性核对的正确范围（附带杀软坑）

修正定义在 `main_menu\common\modifier_type_definitions\`（本机 **3 个文件、2437 条**）。核对时**只读这 3 个文件**——**不要 `Get-ChildItem -Recurse` 扫整个 Steam 游戏目录**，实测被 Windows 杀软拦下两次（脚本在 Steam 库目录递归读 30+ 文件 + 大量正则，触发文件扫描启发式）。同理，数原版数据时用**只读指定文件**的方式，别递归。

## 十一、审查脚本自身的误报（实测 7 次，全部是脚本 bug 不是 mod bug）

写自动审计脚本时，以下每一条都真的产生过假失败：

1. **比对本地化语言头要带尾冒号**——`^l_english$` 漏掉 `:` 会报 22 个假失败。正确：`^l_english:`。
2. **正则要先剥注释行再匹配**——否则会匹配到脚本自己的注释文本（注释里写了示例键名），报出"键缺失"。
3. **正则要显式加 `(?m)`**（PowerShell 默认不是多行模式），否则 `^` 只匹配整个字符串开头，命中数恒为 0 或 1。
4. **检查某字段"是否存在"时别搜原始文本**——会命中文件自己的注释（注释里写了 `potential = { ... }` 的示例）。先过滤 `$_.TrimStart() -notmatch '^#'`。
5. **PowerShell 里 `$k:` / `$id:` / `$l:` 会被解析成作用域变量** → 写 `${k}:` / `${id}` / `${l}`。
6. **`Select-String` 管道里 Match 对象没有 `.Path` 属性**（`$_.Matches | ForEach-Object { Split-Path $_.Path }` 静默输出空）→ 改用 `grep` 工具。
7. **数块时要按括号深度或 tab 层级**——只数 `name = {` 会把嵌套块（`country_modifier`、`estate_preferences`、`potential`、`OR`）一起算进去，政策数虚高约 4 倍。

**反面教训**：把这些脚本的结论当成"审计通过"之前，先拿一个**已知正确答案**的样本自测（例如故意写错一个键，看脚本报不报）。假阴性比假阳性危险得多。
