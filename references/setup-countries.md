# setup/countries（国家定义，00_readme.info）

来源：`in_game\setup\countries\00_readme.info`（注意扩展名为 .info，不是 readme.txt）

## 格式

```
TAG = {
    color = hsv360 { 360 100 100 }
    color2 = hsv360 { 0 0 0 }
    male_regnal_names = {}
    female_regnal_names = {}
    description_category = <administrative>   # 参照 common/country_description_category DB
    difficulty = 2                            # int，1–5
}
```

## 审查要点

- `description_category` 引用须在 `in_game\common\country_description_categories` 存在。
- `difficulty` 取值 1–5。
- 该文件是 .info 扩展名（非 .txt），审查文件发现时应包含。
- 未在 readme 中说明：本地化键格式。
