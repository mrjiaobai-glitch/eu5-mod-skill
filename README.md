# EU5 Mod Skill（EU5 模组制作与审查技能包）

Europa Universalis V（jomini 引擎）模组**制作 + 审查一体**的 agent 技能包。全部知识基于游戏本体目录（`game\in_game`、`game\loading_screen`、`game\main_menu`、`game\dlc`）实际文件提炼，并对照本机已装 mod 验证。

## 结构

```
eu5-mod-skill/
├── SKILL.md                    # 技能主文件：制作工作流（6 步）+ 审查流程（9 项检查清单）
└── references/                 # 知识库（75 个文档）
    ├── README.md               # 索引（字段权威库 + 制作层知识库映射）
    ├── common-*.md             # 字段权威库：74 个 readme 提炼（每类目字段/类型/取值/作用域/坑）
    ├── audit-ids.md            # 引用类 ID 核对规则
    ├── blades-and-thrones-2026-08.md  # 实测坑（EU5 1.3.x）
    ├── game-layout.md          # 游戏本体四区结构地图
    ├── mod-skeleton.md         # mod 骨架（metadata.json 注册）
    ├── merging.md              # INJECT/REPLACE 合并体系
    ├── event-making.md         # 事件制作模板（真实样例）
    ├── scripting-core.md       # script_values / scripted_effects / on_action
    ├── systems-map.md          # common ~160 类目地图
    ├── defines.md              # defines N 块索引
    ├── localization.md         # 本地化（BOM / 键前缀 / 镜像）
    ├── testing.md              # 测试系统与 error.log 排错
    └── pitfalls.md             # 实测坑速查
```

## 使用

作为 DSH/Claude 等 agent 的 skill 目录使用：将 `SKILL.md` 与 `references/` 放入 `~/.dsh/skills/eu5-mod-review/`（或对应 agent 的 skills 目录），agent 即可按"制作 → 测试 → 自检"全流程工作。

## 关键铁律

1. 一切以游戏本体为准：写任何词条前先 grep 游戏本体确认，禁止凭 EU4 记忆补。
2. EU5 ≠ EU4：无 ROOT/PREV、无 event_target、无 `KEY:0`、选项无 weight、作用域词 root/prev/this。
3. 字段不确定 → 查 readme：73 个 `readme.txt` + `_script_values.info` + `on_actions.info` + `_game_rules.info` 是官方权威说明。
4. 制作完成后用审查流程自检。
