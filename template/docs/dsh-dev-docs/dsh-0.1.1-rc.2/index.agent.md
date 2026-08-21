# Agent 阅读指南 —— dsh 插件开发文档（v0.1.1-rc.2）

本文档是给 Agent（自动编程代理）看的使用索引：说明这份文档集装了什么、各篇回答什么问题、在涉及 dsh 插件开发的常见任务中该查哪篇。用户一般不读本文档。

## 这份文档集是什么

本目录 `docs/dsh-dev-docs/dsh-0.1.1-rc.2/` 收录了 DeepSeek-Harness 官方仓库 `docs/user/develop/` 在版本 0.1.1-rc.2 时期的中文版插件开发文档（共 9 篇 + 本索引；英文原版见各 `.md`，中文见 `.zh.md`）。它只覆盖 `docs/user/develop/` 路径；原文档中指向仓其他位置的链接（根 `README.zh.md`、`cookbook/`、`subsystems/`、`apps/cli/`、`packages/`、`capability-seams.md` 等）未随本目录下载，需要时回[官方仓库](https://github.com/deepseek-ai/deepseek-harness)查看。

## 速查：什么任务查哪篇

| 任务 | 查这篇 |
|:---:|:---|
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

- **插件本质**：一个导出 `apply(ctx)` 的 TypeScript 模块，`ctx` 是上下文，通过它注册能力。三种形态：函数、对象、类；一般用函数形式即可，需要向其他插件提供服务时用类形式（`extends Service`）。
- **工具**：`ctx.tools.register(defineTool({ name, description, parameters, output, execute }))`。`parameters` 定义入参 schema，`execute` 返回 `output.schema` 声明的规范值，`output.render` 把值转成面向模型的内容。需要 `inject: ['tools']`。
- **配置**：导出同名 `Config` 类型 + Schemastery schema（`Schema.object({...})`），默认值写进 schema；不能导出普通对象。设计原则：凡不同部署取值可能不同的参数都必须定义为配置字段（无硬编码可调参数）；在 schema 中表达完备约束，使无效配置在插件加载时响亮失败。配置变更会触发 HMR（卸载旧实例、加载新实例，注册随 effect 自动清理）。
- **打包**：组合包（bundle）是附带一个配置层的 npm 包，manifest 声明 `dsh.bundle`（指向一个 patch 文件）；profile 是 `$DSH_HOME/profiles/<name>` 下描述可启动组合的目录，manifest 声明 `dsh.profile` 及有序 `bundles`。`dsh plugin --profile <name> add <包>` 安装。层顺序：bundles 列表 → profile 自己的 `cordis.patch.yml` → home 级 → 每个 `--patch` overlay；后应用的层按行胜出（patch 替换整行 `config`，不深度合并）。git 安装只拉源码、需作者提供自包含的 `prepare` 脚本 + 用户授权（pnpm ≥10 需在 profile 的 `pnpm-workspace.yaml` 里 `allowBuilds`）；不想让用户授权则分发 npm 包或 tarball。

### 框架（framework）

- **生命周期**：插件的 Fiber 状态机 `PENDING → LOADING → ACTIVE`（或 `FAILED`），卸载 `ACTIVE → UNLOADING → DISPOSED`。`ctx` 上所有注册（事件监听、工具、LLM 适配器、`ctx.effect` 资源）卸载时按注册逆序自动清理；有顺序依赖的清理必须放进同一个 `ctx.effect()` 返回的处置器中串行等待。`ctx.plugin()` 创建子 Fiber、随父卸载；`await fiber.dispose()` 手动提前终止（移除注册、递归卸载子插件、等待异步清理完成）。
- **服务**：服务是挂在 `ctx` 上的命名能力（`tools`、`llm`、`agents`）。`inject: ['xxx']` 声明必需依赖，`apply` 时保证就绪；可选依赖用 `ctx.get('xxx')`。必需服务消失会触发依赖插件自动 dispose、恢复后自动重载。提供服务：`class MetricsService extends Service { super(ctx, 'metrics') }` + 声明合并扩展 `Context` 类型。`cordis.yml` 支持服务隔离（同一服务多实例，不同插件组看到不同实例）。
- **事件**：`ctx.on`/`ctx.emit`。四种模式：`emit`（广播）、`bail`（短路，第一个非 `null`/`false`/`undefined` 值胜出）、`serial`（顺序执行并等待异步、同短路规则）、`waterfall`（流水线，监听器**必须调用 `next()`**，否则故意短路——用于拦截/网关）。类型安全用声明合并扩展 `Events` 接口。Harness 事件遵循 `namespace/action` 命名；`turn/*`、`step/*`、`tool/call` 等是**持久化会话事件类型**，不是同名 Cordis 事件，观察它们要监听 `session/event`。事件监听器也是效果，卸载自动移除。

### 实战（practice）

- **三层能力**：Service Definition（契约 + Request/Result 类型）/ Service Provider（实现）/ Consumer（暴露为工具）。Provider 和 Consumer 只依赖 Definition、互不依赖。不要预防性拆分；显式优于隐式（用显式的 `resolve(request): Spec` 步骤处理默认值，不在 `run()` 中隐藏 `?? default`）。
- **LLM 适配器**：继承 `LlmAdapter` 覆写 `stream()`（异步生成 `StreamChunk`），`ctx.llm.registerAdapter(['provider'], adapter)`。StreamChunk 协议：`block-start`/`text-delta`（或 `tool-call-delta`）/`block-end` 成对出现，`finish` 必须是最后一个分片、`usage` 在 `finish` 前，`index` 从 0 递增，工具调用用 `CallId`；错误抛带稳定 code 的 `LlmError`；合并 `attributionHeaders()` 并传递 `options.signal`；可覆写 `resolveModel()`、`listModels()`。

## 给你的操作提示

- 写插件代码前先读对应的**基础**篇：里面的代码片段可直接复用。
- 涉及生命周期/依赖/通信时，回查**框架**篇确认机制，避免手写 `removeListener` 之类（框架自动清理）。
- 文档内代码片段大量标了 `ignore-check`，直接用不一定通过类型检查，需结合你所在 repo 的类型环境适配。
- 本目录是静态快照：判断"最新行为"时，如与官方仓库当前 master 有出入，以仓库现状为准。
- 若任务目标是补全本项目的 README《技术文档》，索引页见 `index.zh.md`；本项目开发进程受 `AGENTS.md` 约束。
