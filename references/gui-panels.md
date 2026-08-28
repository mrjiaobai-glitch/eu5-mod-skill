# gui/panels/disaster 与 gui/panels/situation（面板）

覆盖 readme：`in_game\gui\panels\disaster\readme.txt`、`in_game\gui\panels\situation\readme.txt`

## 创建灾难 UI 面板的步骤（readme 声明）

1. 查看 common.gui 了解可用的构建块。
2. 在 disasters 文件夹新建与灾难同名的文件。
3. 新文件结构与现有灾难文件相同，**特别要使用 `type = disaster_panel`**。
4. 建议以包含相似构建块的现有灾难为基础布局。

## 创建局势 UI 面板的步骤（readme 声明）

1. 查看 common.gui 了解可用构建块。
2. 在 situations 文件夹新建与局势同名的文件。
3. 新文件结构与现有局势文件相同，**特别要使用 `type = situation_panel`**。
4. 建议参考 rise_of_the_ottomans 作为基础布局。

## 审查要点

- 面板文件类型必须用 `disaster_panel` / `situation_panel`。
- 未在 readme 中说明：其他字段。
