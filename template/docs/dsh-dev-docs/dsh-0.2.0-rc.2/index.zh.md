# dsh 插件开发文档(v0.2.0-rc.2)

> 面向 Agent 的利用指南与速查:请查看 `index.agent.md`。

[dsh](https://github.com/deepseek-ai/deepseek-harness) 官方插件开发文档的**中文版**合集索引。

本目录存放 DeepSeek-Harness 官方仓库 `docs/user/develop/` 在版本 **0.2.0-rc.2**(npm 发布包版本号,对应官方仓库标签 `dsh-v0.2.0-rc.2`)的中文版(`.zh.md`)文档(英文原版见各 `.md`)。目录内 30 个文件已与该标签逐字节核对一致,作为本项目开发 dsh 插件时的参考指引。

| 分块 | 作用 |
|:---:|:---|
| [基础](basic/) | 从零写第一个插件:工具、配置、打包安装 |
| [框架](framework/) | Cordis 插件模型:生命周期、事件、服务与依赖 |
| [实战](practice/) | 能力三种角色设计、LLM 适配器、按提示词持久化配置插件 |

> 说明:本目录仅收录 `docs/user/develop/` 下的文档。原文档中指向仓其他路径的链接(如 `README.zh.md`、`cordis-tutorial/`、`subsystems/`、`capability-seams.md`、`cookbook/`、`apps/cli/`、`packages/` 等)不在本次下载范围内,如需参考请前往[官方仓库](https://github.com/deepseek-ai/deepseek-harness)查看原路径。
>
> 各篇同目录的 `.i18n.yaml` 是上游仓库的双语一致性记录(按标题分节记录中英两版的哈希),随文档原样收录,与本项目自身的翻译机制(`docs/tech-spec/translation-ini.md`)无关。

## 基础(basic)

面向插件开发入门,对应官方文档 `docs/user/develop/basic/`。

| 文档 | 主题 | 前置依赖 |
|:---:|:---|:---:|
| [index.zh.md](basic/index.zh.md) | 第一个插件:创建最小 Harness 插件并加载到 Web UI。插件本质(导出 `apply` 的 TS 模块)、三种形态(函数/对象/类)、自动清理、依赖声明 | 无 |
| [tool.zh.md](basic/tool.zh.md) | 开发一个工具:用 `defineTool` 定义模型可调用工具,含参数校验、输出 schema 与 render | 基础 index |
| [config.zh.md](basic/config.zh.md) | 插件配置:Schemastery schema 声明显式配置与默认值;设计原则(无硬编码可调参数、配置错误要响亮);配合 HMR | 基础 index |
| [publish.zh.md](basic/publish.zh.md) | 打包与安装:组合包(bundle)与 profile 两种概念、profile 的两种创建入口(`--from-default-profile` 与 `dsh plugin`)、`dsh plugin` 安装、配置层顺序(`dsh.bundle.patch` 支持有序多文件列表,按同一层应用)、表层组合包持有命令行、GitHub 安装与构建脚本(`prepare` + `allowBuilds`)、linked 安装的依赖声明与解析规则(`peerDependencies` + `devDependencies`) | 基础 index、config |

## 框架(framework)

面向 Cordis 插件运行时机制,对应官方文档 `docs/user/develop/framework/`。

| 文档 | 主题 |
|:---:|:---|
| [index.zh.md](framework/index.zh.md) | 插件与生命周期:Fiber 状态机、依赖驱动加载、自动清理机制、嵌套上下文、dispose 语义与 HMR(热替换插件包为 `@deepseek-ai/dsh-hmr`) |
| [service.zh.md](framework/service.zh.md) | 服务与依赖:`inject` 声明、Service 基类、类型声明合并、必需/可选依赖、服务消失行为、服务隔离、内置服务 |
| [events.zh.md](framework/events.zh.md) | 事件系统:`ctx.on`/`ctx.emit` 四种模式(emit/bail/serial/waterfall)、类型安全事件、Cordis 事件与会话记录、监听器作为效果 |

## 实战(practice)

面向具体能力的实现,对应官方文档 `docs/user/develop/practice/`。

| 文档 | 主题 | 前置依赖 |
|:---:|:---|:---:|
| [index.zh.md](practice/index.zh.md) | 能力的三种角色设计:Service Definition / Service Provider / Consumer 三角色拆分与组合,以 Bash 为例详解教程;设计要点(不预防性拆分、显式优于隐式) | 基础、framework/service |
| [llm-adapter.zh.md](practice/llm-adapter.zh.md) | LLM 适配器:接入新的模型提供方,继承 `LlmAdapter` 实现 `stream()`,StreamChunk 协议、GenerateOptions、`resolveModel`/`listModels`、错误处理;官方 DeepSeek 参考适配器使用 Messages API | practice/index |
| [dynamic-cordis.zh.md](practice/dynamic-cordis.zh.md) | 按提示词配置持久化插件:创造模式提供 Plugin Manager 与只读运行时检查,插件配置属于当前 profile、进程重启后保留;示例为接入 Streamable HTTP MCP 服务器(纯配置组合包 patch 插入 `@deepseek-ai/dsh-mcp-client`,经 `plugin_manager install_bundle` 安装,需核对 `application: applied` 与 `restart-required` 状态语义) | practice/index |

## 建议阅读顺序

```mermaid
flowchart LR
    A[basic/index 第一个插件] --> B[basic/tool 开发工具]
    A --> C[basic/config 插件配置]
    B --> D[basic/publish 打包安装]
    C --> D
    D --> E[framework/index 插件生命周期]
    E --> F[framework/service 服务与依赖]
    E --> G[framework/events 事件系统]
    F --> H[practice/index 能力三角色]
    G --> H
    H --> I[practice/llm-adapter LLM 适配器]
    H --> J[practice/dynamic-cordis 持久化插件配置]
```

建议从 **basic/index**(第一个插件)进入,沿「基础 → 框架 → 实战」逐篇推进;框架与实战依赖基础部分的示例项目。
