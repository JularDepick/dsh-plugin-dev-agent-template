<div align="center">

# dsh-plugin-dev-agent-template

[![Version](https://img.shields.io/badge/Version-0.1.0-green)](https://github.com/JularDepick/dsh-plugin-dev-agent-template/tree/v0.1.0)
[![Copyright](https://img.shields.io/badge/Copyright-JularDepick-0066AA)](./COPYRIGHT)
[![License](https://img.shields.io/badge/License-MIT-yellow)](./LICENSE)

[English](./README_en-US.md)
| [简体中文]

</div>

还在总结你自己的 dsh-plugin 模板？

试试这个模板吧，让你的 Agent 帮助你直接做好一切。

[点击这里去快速开始](#快速开始)


## 这个模板在干什么?

本仓库是一个通用 dsh 插件开发空白模板(预设),未绑定具体插件目标。

本仓库的立场: 不做具体的插件模板代码(不提供可直接运行的插件实现),而是设定 dsh 插件开发的**标准**——预置开发预设与初始化提示词,由 **Agent 驱动插件的初始化和开发**(高度面向 Agent 开发);本模板**以 Agent 驱动为核心,不是面向开发者手动使用的**。

它做两件事:一是预置 dsh 插件项目的开发预设;二是提供一份初始化提示词,指导 Agent 把本仓库就地改造为你的具体插件项目。

- `template/` 预设(新项目初始化时释放到仓库根目录):
  - dsh 官方插件开发文档收录:`docs/dsh-dev-docs/<版本>/`(先读 `index.agent.md` 速查表,再按需精读基础篇与框架篇)
  - 仓库级规范 `docs/repo-spec/tag-release-spec.md`(版本号、Tag、Release 命名)与技术规格 `docs/tech-spec/translation-ini.md`(翻译文件规范)
  - 翻译文件预设 `src/translation/`(仅含 `xx-YY.ini` 与基准模板,不含加载器源码)
  - README 模板:`README_zh-CN.md`、`README_en-US.md`(含占位内容与语言互链,初始化时按项目实际替换,并按核心语言确定最终命名)
  - `AGENTS.md` 空白模板与 `version.index.md` 版本索引模板
- 根目录 `init-prompt.md`(初始化提示词):指导 Agent 与用户问答确认设计信息(插件目标、命名 `dsh-<核心名称>-plugin`、目标 dsh 版本、文档语言核心等),并执行就地改造(清除仓库性内容与 `.git/`、释放 `template/`、`git init`、确认内置空白 `AGENTS.md`、搭建骨架、改造 README 与 `AGENTS.md`)。本文件一次性使用,初始化完成后清除。


## 为什么需要这个模板?

- dsh 插件开发有一套完整的约定:插件本质(`name`/`inject`/`apply`)、配置 schema、工具注册、生命周期与服务依赖、事件模式、bundle 打包与层规则,以及版本号、Tag、Release、翻译文件等仓库级规范;从零搭建容易遗漏,项目之间也难以保持一致。
- 模板将这些约定固化为可复用预设,并把初始化过程写成提示词:Agent 按引导在问答中确认设计信息后就地改造,产出符合约定的项目骨架与文档体系;未经你确认,不装依赖、不构建、不测试。
- 新项目继承 `AGENTS.md` 守则体系(源自 AGENTS.md-Best-Practices 最佳实践):初始化时直接使用模板内置的空白模板并填充项目绑定区,后续会话只需阅读 `AGENTS.md` 即可延续开发。
- dsh 官方版本更新频繁,且官方明确声明存在破坏性变更;得益于 Agent 驱动设计,官方文档更新时只需拉取最新官方插件开发文档(`docs/dsh-dev-docs/<新版本>/`),Agent 即可据此迭代适配,无需开发者手动对照文档逐一排查。


## 快速开始

1. 克隆本仓库到新目录(无需手动挑选文件),示例:`git clone https://github.com/JularDepick/dsh-plugin-dev-agent-template dsh-<核心名称>-plugin`
2. 新开一个 Agent 会话并指向该目录,先让会话完整阅读根目录 `init-prompt.md`。
3. Agent 按提示词执行初始化(涉及删除与 git 操作会先与你确认):清除仓库性内容(含 `.git/`)、释放 `template/`、`git init`、确认内置空白 `AGENTS.md`,并在问答确认核心信息后搭建 `src/` 骨架、补齐打包配置、改造 README 与 `AGENTS.md`。
4. 初始化完成后,按提示将工作目录更名为实际插件仓库名称(如 `dsh-github-plugin`),重新指定工作区,并新开会话查看 `AGENTS.md` 继续开发。

> 注意:初始化是一次性就地改造,会删除本仓库的 git 历史与 `init-prompt.md` 本身;如需保留模板原件,请另行克隆一份。


## 许可证

本仓库采用 **MIT许可证** 详见 [LICENSE](./LICENSE) 。

仓库内置的预设内容采用其源仓库的许可证，不受本仓库许可证的约束，详见相关链接。


## 版权声明

Copyright &copy; 2026 JularDepick

详见 [COPYRIGHT](./COPYRIGHT)


## 相关链接

- dsh 官方仓库: https://github.com/deepseek-ai/deepseek-harness
- dsh 官方插件开发文档: https://github.com/deepseek-ai/deepseek-harness/tree/main/docs/user/develop
- 本仓库内置的 AGENTS.md 最佳实践: https://github.com/JularDepick/AGENTS.md-Best-Practices/tree/main/src/develop/general-methodology/JularDepick/AGENTS.md


## 友情链接