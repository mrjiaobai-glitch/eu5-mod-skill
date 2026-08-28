# Mod 骨架（mod-skeleton）

本机 mod 目录：`C:\Users\17453\Documents\Paradox Interactive\Europa Universalis V\mod\`（launcher 加载）。已装 mod 参照：`刀锋与王座`、`尼泊尔王公自用平衡`。

## 目录结构（真实 mod 实查）

```
mod\<mod名>\
├── .metadata\
│   ├── metadata.json      # launcher 注册（必需）
│   └── thumbnail.png      # 预览图
├── in_game\               # 游戏内覆盖/新增
│   ├── common\<类目>\     # 只放要改的类目
│   ├── events\            # 事件文件
│   ├── gui\               # （可选）
│   ├── localization\<lang>\  # 语言文件（见下）
│   └── setup\             # （可选）国家等
└── main_menu\
    ├── common\            # static_modifiers / modifier_type_definitions / modifier_icons / named_colors 等
    ├── gfx\  gui\  setup\ # （可选）
    └── localization\<lang>\
```

## metadata.json（真实样例：刀锋与王座）

```json
{
	"name":	"刀锋与王座",
	"id":	"blades_and_thrones",
	"version":	"0.2.1",
	"supported_game_version":	"1.3.11",
	"short_description":	"佣兵、常备军与国家政治的联动机制",
	"tags":	["Events", "Military", "Gameplay"],
	"relationships":	[],
	"game_custom_data":	{
	}
}
```

## 文件组织规则

1. **只放要改的文件**：同路径同名 = 覆盖/合并原版（见 `merging.md`）；不存在的路径 = 新增。原版没有的类目子目录直接新建即可。
2. **加载顺序 = 文件名**：`00_` 前缀最先加载，数字越小越先；同数字按文件名序。原版惯例：`00_default.txt` / `0_xxx.txt` / `1_xxx.txt` / `2_xxx.txt` / `3_xxx.txt`（如 unit_types：0_tribal → 1_uniques_for_age_x → 2_unlocked_through_tech → 3_special）。mod 文件建议用大数字或 `zzz_` 前缀保证后加载（后加载的普通条目可覆盖先加载的同名条目；INJECT 语义见 merging.md）。
3. **本地化文件命名**：`<主题>_l_<lang>.yml`，主题建议用 mod 名前缀（真实 mod 用 `zzz_blades_and_thrones_l_english.yml`）。
4. **语言目录**：mod 可放 `in_game\localization\<lang>\` 或 `main_menu\localization\<lang>\`（真实 mod 两者都有用；原版主语言文件在 main_menu，**同键时加载顺序后者胜出，注意镜像规则见 `localization.md`**）。
5. **编码**：yml 必须 UTF-8 **BOM**；txt 建议 BOM。
6. **改名/删除原版文件不可取**：用合并前缀或整体覆盖。

## DLC 包（官方 mod 形态）

`game\dlc\D008_fate_of_the_phoenix\`：in_game + loading_screen + main_menu 三分区 + `D008_fate_of_the_phoenix.dlc.json`（元数据，字段同 metadata.json 风格）+ thumbnail.dds + checksum_manifest.txt。`dlc\D000_shared\` 展示最小共享包：`main_menu\common\dlc\`、`main_menu\localization\dlc\<lang>\`。

## 新国家/新地点注意

- 国家定义：`in_game\setup\countries\<地区>.txt`（格式见 `setup\countries\00_readme.info`：TAG = { color = hsv360{...} color2 male_regnal_names female_regnal_names description_category = administrative difficulty = 2 }，difficulty 1–5）。
- 新增国家还要考虑：`main_menu\common\coat_of_arms\`（纹章）、`flag_definitions\`（旗帜）、地图色、`common\country_ranks\`（等级）。
- 地点（location）由地图二进制决定（`in_game\map_data\`），脚本只能引用 `location:<id>`（地点 ID 列表在 `map_data\location_templates.txt` 或 setup 文件）。
