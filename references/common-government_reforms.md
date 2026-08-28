# common/government_reforms（政府改革）

来源：`in_game\common\government_reforms\readme.txt`

## 字段

```
<reform_id> = {
    age = <age key>                  # 可选；可用的时代
    government = <government type key>  # 可选；支持的政体类型
    major = <yes/no>                 # 排他改革；每国只能实施一个 major
    unique = <yes/no>                # 额外 UI 装饰
    block_for_rebel = <yes/no>       # 叛军不能使用
    locked = <trigger>               # 当前是否锁定、不可交互
    male_regnal_names = { ... }      # 可选；该国统治者采用的男性名号列表
    female_regnal_names = { ... }    # 可选；女性名号列表
    potential = <trigger>            # root = country
    allow = <trigger>                # root = country
    years / months / weeks / days = <int>
    on_activate = <effect>           # root = country
    on_fully_activated = <effect>
    on_deactivate = <effect>         # root = country
    country_modifier / province_modifier / location_modifier = <scaled & triggered modifier>
}
```

## 审查要点

- `major = yes` 每国唯一——多个 major 并存是设计错误。
- `government` 引用须在 common/government_types 存在（若有该目录）。
- 未在 readme 中说明：本地化键格式。
