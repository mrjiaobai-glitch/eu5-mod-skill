# common/bureaucracies（政府官僚制）

来源：`in_game\common\bureaucracies\readme.txt`

## 字段

```
<bureaucracy_id> = {
    potential = <trigger>       # 行动是否可能（root = country）
    allow = <trigger>           # 行动能否开始（root = country）
    years / months / weeks / days = <int>   # 完全生效所需时间；修正按完成比例缩放
    on_activate = <effect>      # 选择时触发（root = country）
    on_fully_activated = <effect>  # 100% 时触发（无延迟则立即）（root = country）
    on_deactivate = <effect>    # 移除时触发（root = country）
    neutral_modifier  = <scaled & triggered modifier>  # 施加于整个国家；root = country; scope:maintenance; scope:entrenchment
    positive_modifier = <scaled & triggered modifier>  # 同上
    negative_modifier = <scaled & triggered modifier>  # 同上
    implementation_price = <price>          # 脚本化标准价格，引用 \common\prices\（price:<price_id> 或脚本结果）
    implementation_price_modifier = <script value>  # 乘数；root = country
    removal_price = <price>
    removal_price_modifier = <script value>
    maintenance_price = <price>
    maintenance_price_modifier = <script value>
    on_maintenance_changed = <effect>       # 维护设置改变后次日调用；root = country; scope:old_maintenance; scope:new_maintenance
}
```

## 审查要点

- 三个 modifier 都是 scaled & triggered modifier，且可用 `scope:maintenance` / `scope:entrenchment` 作用域。
- 价格字段引用 `common/prices` 中定义的价格（`price:<price_id>` 语法），价格 ID 须存在。
- 未在 readme 中说明：本地化键格式。
