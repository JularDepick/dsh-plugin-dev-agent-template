# 翻译文件规范（translation-ini）

## 存放位置

翻译文件存放于 `src/translation/` 目录。

## 命名规范

翻译文件命名遵循 `xx-YY.ini` 格式：

- `xx`：语言代码，小写，遵循 ISO 639-1，如 `zh`、`en`
- `YY`：地区代码，大写，遵循 ISO 3166-1 alpha-2，如 `CN`、`US`

示例：`zh-CN.ini`（简体中文）、`en-US.ini`（美式英语）。

## 文件内容

- 使用 INI 格式，UTF-8 编码。
- 翻译 ini 中不使用注释。
- 文件结构包含两个节：
  - `[meta]`：翻译文件的元信息，包含以下键：
    - `for`：本翻译文件对应的插件版本号
    - `lang`：语言代码，与文件名 `xx-YY` 一致
    - `name`：语言显示名（用于 UI 语言切换展示）
    - `endtime`：最近维护时间，遵循守则日期格式 `yyyy-MM-dd HH:mm:ss+HH:mm`
  - `[translation]`：UI 文案键值对，键名使用英文，值使用对应语言的文案。
- 新增翻译文件时，以 `src/translation/.example_zh-CN.ini` 为基准模板（简体中文版复刻），从该模板翻译更新为 `xx-YY.ini`，改写 `[meta]` 各键后翻译 `[translation]` 键值。

## 模板维护流程

- `src/translation/.example_zh-CN.ini` 是翻译文件的基准模板，内容即简体中文版，与 `zh-CN.ini` 保持一致。
- 版本迭代时直接维护该基准模板（补充、调整 `[translation]` 键值），并保持 `zh-CN.ini` 与基准模板一致。
- 模板更新后，其余语言版本（如 `en-US.ini`）从模板翻译更新，保持各语言键集一致。

## 语言切换机制

- UI 预留翻译功能，提供翻译切换入口。
- 切换目标语言时，加载 `src/translation/` 下对应的 `xx-YY.ini` 文件。
- 未命中翻译的文案回退到默认语言（`zh-CN.ini`）。
