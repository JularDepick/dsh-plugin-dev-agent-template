<div align="center">

# dsh-plugin-dev-agent-template

[![Version](https://img.shields.io/badge/Version-0.1.1-green)](https://github.com/JularDepick/dsh-plugin-dev-agent-template/tree/v0.1.1)
[![Copyright](https://img.shields.io/badge/Copyright-JularDepick-0066AA)](./COPYRIGHT)
[![License](https://img.shields.io/badge/License-MIT-yellow)](./LICENSE)

[English](./README_en-US.md)
| [简体中文]

</div>

还在自己搭 dsh 插件模板?

只需克隆本仓库,让 Agent 按一份提示词帮你完成初始化与后续开发。

[点击这里去快速开始](#快速开始)


## 这个模板能为你做什么?

本仓库是一个通用 dsh 插件开发模板:预置了开发所需的规范文档、翻译预设、README 模板与 AGENTS.md 空白模板,并附带初始化提示词。

你只需要与 Agent 对话,确认需求与关键决策(插件目标、命名、目标 dsh 版本、文档语言等),Agent 就会把仓库就地改造为你的插件项目骨架——源码、配置、文档体系一次到位。之后每次新开会话,它读取 AGENTS.md 即可接手继续开发。

## 为什么需要它?

- dsh 插件开发约定繁多,从零搭建容易遗漏;
- 约定已固化为预设与提示词,Agent 按你的决策直接产出符合规范的项目;
- 官方版本更新频繁且有破坏性变更,更新官方文档后 Agent 即可迭代适配;
- 无需人工维护开发记忆:AGENTS.md 守则体系让每个新会话都能接手。

## 快速开始

1. 克隆本仓库:`git clone https://github.com/JularDepick/dsh-plugin-dev-agent-template <插件仓库名>`
2. 新开 Agent 会话指向该目录,告诉 Agent 先读 `init-prompt.md`;
3. 根据 Agent 引导完成后续步骤。

> 注意:初始化会删除本仓库的 git 历史与 `init-prompt.md`;如需保留模板原件,请另行克隆一份。

## 许可证

采用 **MIT License**,详见 [LICENSE](./LICENSE)。内置预设内容沿用其源仓库许可证。

## 版权声明

Copyright &copy; 2026 JularDepick,详见 [COPYRIGHT](./COPYRIGHT)。

## 相关链接

- dsh 官方仓库: https://github.com/deepseek-ai/deepseek-harness
- dsh 官方插件开发文档: https://github.com/deepseek-ai/deepseek-harness/tree/main/docs/user/develop
- AGENTS.md 最佳实践: https://github.com/JularDepick/AGENTS.md-Best-Practices/tree/main/src/develop/general-methodology/JularDepick/AGENTS.md


## 友情链接