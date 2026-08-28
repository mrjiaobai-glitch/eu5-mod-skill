# common/child_educations（儿童教育）

来源：`in_game\common\child_educations\readme.txt`

## 字段

```
<child education id> = {
    modifier = { <character modifiers> }     # 角色修正
    country_modifier = { <country modifiers> }  # 国家修正
    price_to_select = <price>
    price_to_deselect = <price>
    potential = { <character triggers> }
    allow = { <character triggers> }
    on_education_start_effect = { <character effects> }  # 选中时触发
}
```

## 审查要点

- potential/allow 均为**角色作用域** trigger；effect 也是角色作用域。
- 未在 readme 中说明：本地化键格式。
