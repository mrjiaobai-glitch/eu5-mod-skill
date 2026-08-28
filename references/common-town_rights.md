# common/town_rights（城镇权利）

来源：`in_game\common\town_rights\readme.txt`

## 字段

```
<town_right_id> = {
    color = <named color>
    allow = {}          # root = country, target = location
    potential = {}      # root = country
    location_modifier = {}
    country_modifier = {}
}
```

## 审查要点

- `allow` 的 root = country、target = location；`potential` 的 root = country。
- 未在 readme 中说明：本地化键格式。
