# gui/filters（脚本化过滤器）

来源：`in_game\gui\filters\readme.txt`

## 字段

```
<filter_tag> = {
    scope = <scope type>            # 该过滤器可操作的对象类型
    trigger = <trigger>             # 提供的 scope 对象是否通过过滤器
    range = {                       # 提供数值范围选择
        min = <值>
        max = <值>
        step = <值>
        format = <string>           # 显示文本格式
    }
    tag = <string>                  # 管道分隔的标签列表（"a|b|c"）；视图 .gui 用 WithFilterTags('x|y|z') 暴露交集；**省略 tag = 全视图可见**（仅限真正通用的过滤器）
    group = <integer>               # 同一 UI 背景内的分组
    exclusive_group = <integer>     # 单选按钮组；**组内每个过滤器都必须写**；选中一个则取消上一个
    invert = <yes/no>               # 默认排除，勾选后包含
    enabled_at_start = <yes/no>     # 初始勾选状态
    hidden_in_searchbar = <yes/no>  # 仍生效并出现在侧边菜单/自动补全，但搜索栏不显示启用芯片（面板有专用按钮时用，如 estates_filter_button）
    group_sorting = <...>           # 组内排序；目前仅字母序
}
```

## 本地化（readme 声明）

- `search_filter_<tag>_name`：过滤器名
- `search_filter_<tag>_desc`：描述
- `search_filter_<tag>_format`：使用 range 的过滤器显示数字的格式

## 语义（readme 声明）

- 带子项的列表：任一子项通过过滤器则整个项通过。
- 检查顺序：搜索栏过滤器 → 标签 → 分组。

## 审查要点

- `exclusive_group` 必须组内每个过滤器都写，否则单选失效。
- 省略 `tag` 的过滤器会出现在所有视图——滥用是常见错误。
- 未在 readme 中说明：无。
