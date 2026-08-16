# Agent 阅读指南 —— dsh 插件开发文档（v0.1.0-rc.6）

本文档是给 Agent（自动编程代理）看的使用索引：说明这份文档集装了什么、各篇回答什么问题、在涉及 dsh 插件开发的常见任务中该查哪篇。用户一般不读本文档。

## 这份文档集是什么

本目录 `docs/dsh-dev-docs/dsh-0.1.0-rc.6/` 收录了 DeepSeek-Harness 官方仓库 `docs/user/develop/` 在版本 0.1.0-rc.6 时期的中文版插件开发文档（共 9 篇 + 本索引）。它只覆盖 `docs/user/develop/` 路径；原文档中指向仓其他位置的链接（`README.md`、`cookbook/`、`subsystems/`、`apps/cli/`、`packages/`、`capability-seams.md` 等）未随本目录下载，需要时回[官方仓库](https://github.com/deepseek-ai/deepseek-harness)查看。

## 速查：什么任务查哪篇

| 任务 | 查这篇 |
|:---|:---|
| 我要写一个最简单的插件并在 Web UI 里跑起来 | [基础入门](basic/index.zh.md) |
| 我要给插件加一个 Agent 能调用的工具 | [开发一个工具](basic/tool.zh.md) |
| 插件要有用户可改的配置项 | [插件配置](basic/config.zh.md) |
| 要把插件打包成可安装的产物分发给别人 | [打包与安装插件](basic/publish.zh.md) |
| 我要理解插件何时加载/卸载、生命周期的清理机制 | [插件与生命周期](framework/index.zh.md) |
| 插件要暴露服务给别人用 / 依赖别的服务 | [服务与依赖](framework/service.zh.md) |
| 插件之间要松耦合通信 | [事件系统](framework/events.zh.md) |
| 想把能力拆成可替换的提供方 | [能力的三种角色](practice/index.zh.md) |
| 要接入一个新的模型提供方（LLM） | [LLM 适配器](practice/llm-adapter.zh.md) |

## 各篇核心结论（速记）

### 基础（basic）

- **插件本质**：一个导出 `apply(ctx)` 的 TypeScript 模块，`ctx` 是上下文，通过它注册能力。三种形态：函数、对象、类；一般用函数形式即可。
- **工具**：`ctx.tools.register(defineTool({ name, description, parameters, output, execute }))`。`parameters` 定义入参 schema，`execute` 返回规范值，`output.render` 把值转成面向模型的内容。需要 `inject: ['tools']`。
- **配置**：导出同名 `Config` 类型 + Schemastery schema（`Schema.object({...})`），默认值写进 schema；不能导出普通对象。无效配置会在加载时响亮失败。
- **打包**：`dsh.bundle` 声明组合包（贡献一个 patch 层）；`dsh.profile` + `bundles` 描述 profile 由哪些包按序组成。用 `dsh plugin --profile <name> add <包>` 安装。配置层后应用的层按行胜出（整行替换，不深度合并）。

### 框架（framework）

- **生命周期**：插件的 Fiber 状态机 `PENDING → LOADING → ACTIVE → UNLOADING → DISPOSED`（或 `FAILED`）。`ctx` 上所有注册卸载时自动清理；手动资源用 `ctx.effect(() => cleanup)`。
- **服务**：服务是挂在 `ctx` 上的命名能力（`tools`、`llm`、`agents`）。用 `inject: ['xxx']` 声明必需依赖，`apply` 时保证就绪；可选依赖用 `ctx.get('xxx')`。服务消失会触发依赖它的插件自动 dispose 并随后重载。
- **事件**：`ctx.on`/`ctx.emit`。四种模式：`emit`（广播）、`bail`（短路返回）、`serial`（顺序+短停）、`waterfall`（流水线，监听器必须 `next()`）。监听器也是效果，卸载自动移除。

### 实战（practice）

- **三层能力**：Service Definition（契约+类型）/ Service Provider（实现）/ Consumer（暴露为工具）。定义只在 Service Definition 中，Provider 和 Consumer 都只依赖 Definition，互不依赖。不要预防性拆分。
- **LLM 适配器**：继承 `LlmAdapter` 覆写 `stream()`（异步生成 `StreamChunk`），`ctx.llm.registerAdapter(['provider'], adapter)`。协议要点：`block-start`/`text-delta`/`block-end` 成对出现、`finish` 必须是最后一个分片、`usage` 在 `finish` 前、错误抛带稳定 code 的 `LlmError`。

## 给你的操作提示

- 写插件代码前先读对应的**基础**篇：里面的代码片段可直接复用。
- 涉及生命周期/依赖/通信时，回查**框架**篇确认机制，避免手写 `removeListener` 之类（框架自动清理）。
- 文档内代码片段大量标了 `ignore-check`，直接用不一定通过类型检查，需结合你所在 repo 的类型环境适配。
- 本目录是静态快照：判断"最新行为"时，如与官方仓库当前 master 有出入，以仓库现状为准。
- 若任务目标是补全本项目的 README《技术文档》，索引页见 `index.zh.md`；本项目开发进程受 `AGENTS.md` 约束。
