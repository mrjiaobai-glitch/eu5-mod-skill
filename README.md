# EU5 Mod Skill（EU5 模组制作与审查技能包）

Europa Universalis V（jomini 引擎）模组**制作 + 审查一体**的 agent 技能包，另含**原版机制解析库**。全部知识基于游戏本体目录（`game\in_game`、`game\loading_screen`、`game\main_menu`、`game\dlc`）实际文件提炼，并对照本机已装 mod 与玩家实测验证。

## 结构

```
eu5-mod-skill/
├── SKILL.md                    # 技能主文件：制作工作流（6 步）+ 审查流程（9 项检查清单）
└── references/                 # 知识库
    ├── README.md               # 索引（四类文档映射 + 铁律）
    ├── common-*.md             # ① 字段权威库：74 个 readme 提炼（每类目字段/类型/取值/作用域/坑）
    ├── audit-ids.md            # 引用类 ID 核对规则
    ├── game-layout.md          # ② 制作层知识库
    ├── mod-skeleton.md         #    mod 骨架（metadata.json 注册）
    ├── merging.md              #    INJECT/REPLACE 合并体系
    ├── event-making.md         #    事件制作模板（真实样例）
    ├── scripting-core.md       #    script_values / scripted_effects / on_action
    ├── systems-map.md          #    common ~160 类目地图
    ├── law-design.md           #    法律设计与落地
    ├── defines.md              #    defines N 块索引
    ├── localization.md         #    本地化（BOM / 键前缀 / 镜像）
    ├── testing.md              #    测试系统与 error.log 排错
    ├── pitfalls.md             #    实测坑速查
    ├── vanilla-weather.md      # ③ 原版机制解析库
    ├── vanilla-combat.md       #    战斗系统
    ├── vanilla-pop.md          #    POP 与阶层
    ├── vanilla-mandate-of-heaven.md  # 天命与天朝 IO
    ├── vanilla-tech-and-age.md #    科技与时代（革新/思潮/时代）
    ├── blades-and-thrones-2026-08.md          # ④ 实测经验
    └── laws-events-and-estates-2026-09.md     #    2026-09 复核实测
```

## 使用

作为 DSH/Claude 等 agent 的 skill 目录使用：将 `SKILL.md` 与 `references/` 放入 `~/.dsh/skills/eu5-mod-review/`（或对应 agent 的 skills 目录），agent 即可按"制作 → 测试 → 自检"全流程工作。原版机制解析（`vanilla-*.md`）也可单独当 EU5 机制手册阅读——每篇都标注了实查文件、行号、**脚本可改点与引擎硬编码边界**。

## 关键铁律

1. 一切以游戏本体为准：写任何词条前先 grep 游戏本体确认，禁止凭 EU4 记忆补。
2. EU5 ≠ EU4：无 ROOT/PREV、无 event_target、无 `KEY:0`、选项无 weight、作用域词 root/prev/this。
3. 字段不确定 → 查 readme：73 个 `readme.txt` + `_script_values.info` + `on_actions.info` + `_game_rules.info` 是官方权威说明。
4. 命名先查撞名：新增法律/政策/建筑/改革前，显示名与键名都要对照本体 `main_menu\localization\simp_chinese\` 查一遍。
5. 数值先查分布：新增修正值前先抽样本体该修正的取值分布、吸附到实际出现过的精确值。
6. 制作完成后用审查流程自检。

## 原版机制解析已覆盖

| 系统 | 要点 |
|---|---|
| 天气与气候 | 8 气候带 + 引擎天气实体（锋面/气旋/龙卷风）、`weather_monthly_pulse` 生成分支、地形衰减字段原版全 0（可改空档） |
| 战斗 | 6 时代兵种模板数值、NCombat 全常量、地形 `defender` 骰子加成、围城三部曲、`on_battle_*` scope |
| POP 与阶层 | 8 种 POP 类型、四循环（增长/晋升/迁移/同化改宗）、满意度与叛乱阈值、阶层三套修正块 |
| 天命与天朝 IO | 宣称天命 CB vs 夺取天命和约、三条入会路径、天朝威仪与中华王朝危机、朝贡体系 |
| 科技与时代 | 6 时代表与推进机制、专精三选一、思潮（institution）诞生与 9 条传播通道、革新 3178 条与研究进度四要素 |
