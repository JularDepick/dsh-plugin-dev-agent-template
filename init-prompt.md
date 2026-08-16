# init-prompt(模板初始化提示)

本仓库是通用 dsh 插件开发空白模板(预设),未绑定具体插件目标。本文件是"初始化提示词",一次性使用:用户克隆本仓库后,新项目中的 Agent 会话先读本文件,在与用户的问答中确认设计信息,把本仓库就地调整为适合用户实际项目的插件仓库;初始化完成后清除本文件(见"五、初始化收尾"),后续会话直接查看 `AGENTS.md`。

## 一、开场提示(阅读本文件时第一件事)

- 告知用户:本仓库是用户克隆来的模板仓库;用户要求阅读本初始化提示词时,对仓库执行本文件所述全部操作。
- 说明操作会清除仓库性内容与 `.git/` 并就地改造仓库,涉及删除与重建 git 历史,动手前先与用户确认。
- 本文件是一次性的:只用于第一次初始化会话;初始化完成后清除(见"三"步骤 8),本仓库随即从模板变为适应具体项目的插件仓库,应提醒用户把工作目录更改为实际插件仓库名称并新开会话(见"三"步骤 9)。

## 二、模板定位与预设

- `template/` 下已提供(初始化时释放到仓库根目录):
  - dsh 官方插件开发文档收录:`template/docs/dsh-dev-docs/<版本>/`(先读 `index.agent.md` 速查表,再按需精读基础篇与框架篇)
  - 仓库级规范 `template/docs/repo-spec/tag-release-spec.md`(版本号、Tag、Release 命名)
  - 技术规格 `template/docs/tech-spec/translation-ini.md`(翻译文件规范)与翻译文件预设 `template/src/translation/`(仅含 `xx-YY.ini` 与基准模板,不含加载器源码)
  - README 预设:`template/README_zh-CN.md`、`template/README_en-US.md`(中英双语模板,含占位内容与语言互链;初始化时按项目实际替换并按核心语言更名,见"三")
  - `AGENTS.md` 空白模板与 `version.index.md` 版本索引模板:`template/AGENTS.md`、`template/version.index.md`(初始化时直接使用并改造)
- `AGENTS.md` 空白模板已收录于 `template/AGENTS.md`(已下载的最新空白模板,避免初始化时网络下载失败;守则区为硬约束,项目绑定区随新项目状态懒维护)。
- 模板未提供且需在初始化阶段补齐:插件入口、配置、全局常量、翻译加载器、打包配置、`.gitignore` 等(README、AGENTS.md、version.index.md 已由 template/ 预设提供),见下文初始化流程。
- 维护约定:`template/` 下内容保持预设状态,`template/AGENTS.md` 收录空白模板(不写入任何项目绑定内容);新项目初始化时直接使用它生成自身 `AGENTS.md`,仅由新项目填充其项目绑定区。
- `AGENTS.md` 空白模板直接使用 `template/AGENTS.md`:模板仓库已收录,初始化时随 `template/` 释放到根目录,无需网络下载(下载易遇网络问题)。如需获取更新版本,按 `AGENTS.md` 守则区"更新模板版本"流程执行(用户明确要求时),更新后把空白模板同步回模板仓库 `template/AGENTS.md`。模板版本以模板守则区标注为准;模板仓库不向 `template/AGENTS.md` 写入任何项目绑定内容。
- 本文件(`init-prompt.md`)是一次性的:仅用于第一次初始化会话;初始化完成后须从新项目清除,本仓库随即从模板变为适应具体项目的插件仓库,应提醒用户重命名工作目录并新开会话(见"三"步骤 9)。模板仓库(克隆源)自身的后续维护会话,以本文件为维护约定参考,不执行初始化流程。

## 三、初始化流程(就地改造,一次性)

用户克隆本仓库后,Agent 按以下顺序执行(涉及删除与 git 操作处先与用户确认):

1. 开场提示:告知用户本仓库是克隆来的模板仓库,将按本文件执行全部初始化操作;确认后开始。
2. 清除仓库性内容:只保留 `template/` 目录与本文件 `init-prompt.md`,删除其余一切(如 `LICENSE`、`.gitignore`、`.git/` 等;README、AGENTS.md 等预设均位于 `template/` 内,不在此列)。删除 `.git/` 即放弃模板仓库的 git 历史,新项目将建立独立历史。
3. 释放 `template/`:把 `template/` 下所有内容移动到仓库根目录(如 `template/docs/` 移为 `docs/`、`template/src/` 移为 `src/`),随后删除空的 `template/` 目录。
4. 初始化 git 状态:在根目录 `git init` 重新建立版本控制;询问用户是否已有在线仓库地址(如有则记录远程地址,后续 push 时配置 remote origin;没有则告知用户可后续创建,不擅自配置);初始提交时机与内容按用户意愿决定(见"九、守则要点提醒":git 写入需当次授权)。
5. 确认 `AGENTS.md` 空白模板:`template/AGENTS.md` 已在步骤 3 释放到根目录(模板仓库已收录,无需网络下载);如需更新模板版本,按 `AGENTS.md` 守则区模板更新流程执行(用户明确要求时)。
6. 与用户确认新项目核心信息与关键技术决策,并搭建项目骨架:
   - 核心信息:插件目标(要做什么)、包名与简称(插件名称规范:`dsh-<核心名称>-plugin`,如 dsh-github-plugin,与用户确认核心名称)、目标 dsh 版本(决定 Release 命名,见 repo-spec)、项目文档语言核心(询问用户,默认 `README.md` 为中文,英文作为额外文档)。
   - 关键技术决策:交付形式(纯源码骨架 vs 可打包 bundle)、包管理器与构建工具(dsh 生态默认 pnpm + tsdown)、本次是否安装依赖与运行验证。未经授权不装依赖、不构建、不测试。
   - 搭建 `src/` 骨架(翻译预设已随释放就位,其余源码按本步骤生成):
     - 入口 `src/index.ts`:导出 `name`、`inject`、`apply(ctx, config)`,装配各模块。
     - 配置 `src/config.ts`:`Config` 接口 + Schemastery `Schema`,默认值写入 schema。
     - 常量 `src/constants.ts`:全局常量与设计细节(默认语言、回退语言、翻译文件目录等)隔离存放;设计细节同时登记到 `AGENTS.md` 设计细节段。
     - 翻译模块(`src/translation/` 现仅含 `xx-YY.ini` 与基准模板,加载器需生成):
       - 生成 `src/translation/index.ts`:按 locale 加载对应 `xx-YY.ini`、解析 `[translation]` 节键值、未命中回退默认语言(`zh-CN`)、提供语言切换入口;涉及的常量(回退语言、翻译目录)引用 `src/constants.ts`。
       - 键值与加载行为遵循 `docs/tech-spec/translation-ini.md`;文件缺失时回退空表,由主逻辑兜底。
     - 按新项目机制规划业务模块目录(如认证/凭证/工具等,以项目机制为准),模块内拆分类型定义与实现,占位方法以抛错或空值标明"尚未实现"。
   - 补齐打包配置:根目录 `package.json`(声明 `dsh.bundle`、`main/types`、`files`)、`cordis.patch.yml`(patch 层插入插件行)、`tsconfig.json`、`tsdown.config.ts`。
   - 初始化项目文档体系:更新 `AGENTS.md` 的项目绑定区(概述/技术栈/目录结构/设计细节/版本号索引);完善 `version.index.md`(模板已随释放就位:记录项目当前版本号,并列出版本号迭代需同步更新的文件清单,含文件路径与行号;初始版本号按 repo-spec 规范确定起始值,如 `v0.1.0`,建立后向用户报告并确认,再登记进 AGENTS.md 版本号索引段);**询问用户是否添加 `.gitignore`**(按用户意愿建立;库包惯例:忽略 `node_modules/`、`.pnpm-store/`、`*.tgz`、产物目录等;版本文档占用 `v*-*.md` 规则;不做 IDE 类多余预留);按需增补 `docs/tech-spec/` 的机制规格文档;`LICENSE` 按用户意愿重建或延后。
   - 改造 README 预设:替换 `README_zh-CN.md`、`README_en-US.md` 中的占位内容(项目名、简介、特性、作者、仓库地址、许可证等)并补全正文,使其符合用户项目实际;按用户指定核心语言(默认中文)将对应语言版本更名为 `README.md`,另一语言版本保留 `README_<语言>.md` 命名;语言互链同步更新为更名后的文件名;README 命名与改造结果登记到 AGENTS.md 目录结构与 version.index.md 同步清单。询问用户是否乐意支持模板推广:若支持,在 README「相关链接」中新增条目「本仓库使用的插件模板: https://github.com/JularDepick/dsh-plugin-dev-agent-template」;不支持则不添加。
   - 改造 `AGENTS.md` 空白模板:
     - 替换作者信息:模板守则区中模板作者占位(如"项目作者 [JularDepick](https://github.com/JularDepick)")替换为用户的实际 GitHub 身份;
     - 填充项目绑定区:概述/技术栈/架构/目录结构/工作流程/开发时配置文件/设计细节/版本号索引/快捷命令等,按问答结论逐步填充(落点见"五、初始化收尾"迁移映射);
     - 守则区为硬约束:不修改守则内容与模板版本号,除非用户明确要求维护守则。
   - 核对 dsh 插件开发者文档是否过时:将已收录版本(`docs/dsh-dev-docs/<版本>/` 中的版本号,即释放后的模板收录版本)与官方仓库 `docs/user/develop` 的最新文档/发布版本对照,提醒用户确认或由 Agent 自行确认;过时则按官方收录流程更新到 `docs/dsh-dev-docs/<新版本>/`。
   - 最小验证:实现最小真实链路(翻译加载、配置默认值),经用户授权安装依赖、构建、冒烟测试,确认骨架可运行后再进入功能实现。
7. 根据向用户的询问和回复,维护 `AGENTS.md` 绑定具体实践场景:项目绑定区随问答结论逐步填充,直到仅凭 `AGENTS.md` 即可延续开发(见"五、初始化收尾")。
8. 清除 `init-prompt.md`:初始化完成且自查通过后,删除本文件。
9. 收尾提醒:工作流程结束,①提醒用户将工作目录名称更改为实际插件仓库名称(`dsh-<核心名称>-plugin`),重新指定工作区为更改后的工作目录,然后新开会话(新会话查看 `AGENTS.md` 即可延续开发);②提醒用户项目当前缺少 `COPYRIGHT` 与 `LICENSE` 文件(只提醒,不自动创建,按用户意愿后续补齐)。

## 四、项目启动流程(后续会话接手已初始化的项目)

初始化完成、init-prompt.md 已清除后,后续新会话按此流程启动(本流程内容已在迁移时并入 AGENTS.md):

1. 完整阅读根目录 `AGENTS.md`:守则区为硬约束;项目绑定区(概述/技术栈/目录结构/设计细节/版本号索引)随项目状态懒维护。
2. 读 `version.index.md` 与 git log,确认当前真实版本号,向用户汇报确认;版本格式遵循项目内 repo 规范(通常位于 `docs/repo-spec/tag-release-spec.md`)。
3. 读核心 README(中文版)与项目内技术文档(如 `docs/tech-spec/`),掌握既定机制设计;机制细节遵循对应 specs,不重复抄录。
4. 读项目内已收录的 dsh 插件开发文档(通常位于 `docs/dsh-dev-docs/<版本>/`):先读 `index.agent.md` 速查表,涉及框架机制时精读基础篇与框架篇;未收录时回官方仓库 `docs/user/develop` 查阅。
5. 动手前的关键技术决策先列给用户裁决;涉及安装/构建/测试须经授权。

## 五、初始化收尾:内容迁移持久化到 AGENTS.md

初始化完成后,把 init-prompt.md 的成果与可复用经验迁移进新项目 AGENTS.md 项目绑定区,使后续新对话继承仓库时只读 AGENTS.md 即可延续开发。迁移映射:

| init-prompt 内容 | AGENTS.md 落点 |
|:---|:---|
| 新项目定位、插件目标 | 概述 |
| 插件名称规范(`dsh-<核心名称>-plugin`,如 dsh-github-plugin) | 概述或设计细节 |
| dsh 插件技术栈(含确认后的包管理/构建选型) | 技术栈 |
| 实际 src/ 结构与根目录文件 | 目录结构 |
| 打包相关配置文件 | 开发时配置文件 |
| 语言/翻译约定、全局常量索引 | 设计细节 |
| 项目文档语言核心(默认 `README.md` 为中文,英文作为额外文档) | 项目绑定区(概述或设计细节) |
| 当前版本与 version.index.md 同步清单 | 版本号索引 |
| install/build/pack/冒烟命令 | 快捷命令 |
| 插件开发经验与构建测试经验(本文六、七部分) | 浓缩并入项目绑定区(可新增"开发经验"小节,保留全部关键要点) |
| 项目启动流程(本文四部分) | 并入项目绑定区(如"项目启动"小节) |
| 维护规则:按需检查 dsh 插件开发者文档是否过时,过时则按官方收录流程更新到 `docs/dsh-dev-docs/<新版本>/` | 项目绑定区(设计细节或维护规则相关段落) |
| template/ 预设文件的使用说明(README 各语言版本、AGENTS.md、version.index.md、docs/ 文档体系、src/translation/ 翻译预设) | 目录结构、开发时配置文件、设计细节(可变更区域,非守则区域) |

迁移执行时:

- 全量加载 AGENTS.md 后改写;允许转述、浓缩、适当取舍(如模板使用说明、初始化一次性细节),但不得丢失可复用经验与项目决策记录。
- 关键要点必须保留:插件本质(name/apply/inject)、配置 schema、工具注册、生命周期与服务依赖、事件模式、bundle 打包与层规则、版本对齐、缓存重定向、产物后缀、file:// import、冒烟测试套路,以及本项目实际选型与路径。
- 维护规则持久化:把"按需检查 dsh 插件开发者文档是否过时,过时则按官方收录流程更新到 `docs/dsh-dev-docs/<新版本>/`"这一维护规则写入新项目 AGENTS.md 项目绑定区(如设计细节或维护规则相关段落),使其成为持久化规则,后续会话只读 AGENTS.md 即可遵循。
- version.index.md 说明注入:把 version.index.md 的定位与维护方式写入 AGENTS.md 版本号索引段——记录项目当前版本号,并列出版本号迭代需同步更新的文件清单(文件路径与行号);读取时先查看 git 历史、配置文件、关键文档,向用户汇报确认真实版本号,需要时更新;版本号迭代时按清单同步更新所列文件中的版本号,允许继续新增清单项;版本号格式遵循 `docs/repo-spec/tag-release-spec.md`。使后续会话只读 AGENTS.md 即可知晓 version.index.md 的用途与维护规则。
- 文档语言核心决策持久化:把"项目文档语言核心"询问的结论(默认 `README.md` 为中文,英文作为额外文档)写入新项目 AGENTS.md 项目绑定区(如概述或设计细节段落),后续会话据此维护文档语言布局。
- 插件命名规范持久化:把 `dsh-<核心名称>-plugin` 命名规范(含确认后的核心名称)写入新项目 AGENTS.md 项目绑定区(如概述或设计细节段落),后续会话据此命名与新增模块。
- AGENTS.md 改造核对:初始化收尾时确认守则区模板作者已替换为用户实际身份、项目绑定区已按迁移映射填充完整;模板版本号保留。
- template/ 预设文件使用注入:初始化收尾时,把释放后的各预设文件的用途与使用方式写入 AGENTS.md **项目绑定区(可变更区域,非守则区域)**:根目录文件与结构(README 各语言版本及其命名、AGENTS.md、version.index.md、.gitignore 等)登记到目录结构段;打包与配置文件(package.json、cordis.patch.yml、tsconfig、tsdown.config)登记到开发时配置文件段;文档体系(`docs/dsh-dev-docs/<版本>/` 先读 `index.agent.md`、`docs/repo-spec/`、`docs/tech-spec/`)与翻译预设(`src/translation/`)的使用方式登记到设计细节或对应段落。守则区不注入任何项目内容。
- 迁移完成后自查:仅凭 AGENTS.md 能否复现本文件的指导作用(含项目启动流程与全部经验);能则**必须清除新项目中的 init-prompt.md 本身**(即"三"步骤 8),并同步更新 AGENTS.md 目录结构(如已添加该行则移除)。
- 本迁移流程只在新项目执行,模板仓库(克隆源)本体不适用:模板仓库不向 `template/AGENTS.md` 写入项目绑定内容,`template/` 内容保持预设,init-prompt.md 保留作为维护约定。
- 版本计划、版本文档等后续产物按守则写入对应文档,不再依赖 init-prompt.md。

## 六、dsh 插件开发经验速查

- 插件本质:导出 `apply(ctx)` 的 TypeScript 模块;`ctx` 是上下文,经它注册能力(工具、事件、资源)。三种形态(函数/对象/类),一般函数形式即可。
- 插件名:导出 `name`;必需依赖用 `export const inject = ['services...']` 声明,框架保证依赖就绪后才执行 `apply`。
- 配置:导出同名 `Config` 类型 + Schemastery `Schema`(默认值写入 schema);无效配置在加载期响亮失败;不导出普通对象。
- 工具:经 `ctx.tools.register(defineTool({ name, description, parameters, output, execute }))`;`output.render` 把规范值转成面向模型的内容;需要 `inject: ['tools']`。
- 生命周期:插件 Fiber 状态机;经 `ctx` 的注册在卸载时自动清理;手动资源用 `ctx.effect(() => cleanup)`。
- 服务与依赖:服务是挂在 `ctx` 上的命名能力;`inject` 声明必需依赖,可选依赖用 `ctx.get()`;服务消失会触发依赖插件自动卸载并在恢复后重载。
- 事件:`ctx.on`/`ctx.emit`,四种模式(emit 广播/bail 短路/serial 顺序/waterfall 流水线,waterfall 监听器必须调用 `next()`);类型安全用声明合并扩展事件接口。
- bundle 打包:包清单声明 `dsh.bundle` 与 patch 层;patch 以插件包名插入插件行,加载顺序按 profile bundles 列表;后应用的层按行胜出(整行替换,不深度合并)。`dsh plugin --profile <name> add <包>` 安装;git 安装只拉源码,需 `prepare` 脚本且用户授权构建。
- 目录组织:入口、配置 schema、全局常量(设计细节)独立成文件,机制按模块分目录,模块内拆分类型定义与实现;占位方法以抛错或空值标明"尚未实现"。
- 代码规范:代码英文、注释中文(跨行注释)、无 emoji、对象封装、可复用模板提成独立文件、后端文件注释头标注作者。

## 七、构建与测试经验(Windows + 沙箱环境)

- 依赖版本对齐:先查本地已装 dsh 各包版本(`node_modules/@deepseek-ai/*/package.json`),与 npm registry 发布版本比对,对齐到本地运行版本(带 rc 的包需核对 registry 的 next 标签)。
- npm/pnpm 写缓存到工作区外会被拒(EPERM):store/cache/state 重定向到工作区内(如 `.agent/`);pnpm 曾把内容寻址 store 落到工作区根 `.pnpm-store/`,记得加入 .gitignore。
- tsdown 产物为 `.mjs/.d.mts`,`package.json` 的 `main/types` 必须与真实产物对齐。
- Node 动态 import 绝对路径必须转 `file://`(Windows 报 ERR_UNSUPPORTED_ESM_URL_SCHEME)。
- 冒烟测试(临时脚本放 `.agent/`):对构建产物断言入口导出、配置默认值、假 ctx 验证装配与工具注册、翻译加载回退;`pnpm pack` 后列 tarball 内容核对打包边界(`files` 收窄,避免源码混入)。
- PowerShell 每次调用独立无状态,必要时传 `workdir`;控制台中文乱码不代表文件损坏(UTF-8 正常)。

## 八、下一步开发建议

- 顺序建议:先确认骨架可运行(构建/打包/冒烟通过),再按计划逐块实现机制功能;每块完成后同步更新 `AGENTS.md` 目录结构与文档。
- 需要写版本计划时,按守则以目标版本号和日期为索引写入 `AGENTS-UPDATES-PLAN.md`(用户要求放项目内才创建)。
- 需要写版本设计/规范文档时,按守则模板在 `docs/` 下创建 `v版本号-*机制(与规范).md`,做抽象级机制总结(不写具体实现代码与变量名),已有主题优先相对链接引用。
- 涉及秘密(API Key、私钥、令牌)按用户约定处理,无法获取的先留空,不编造。

## 九、守则要点提醒

- 沟通与注释用中文;文档与代码不用 emoji;维护 md 全量加载避免遗漏,允许改写不得丢失细节。
- git 写入(add/commit/push/tag/release)需用户当次授权;staging 只允许 `git add .`;未授权不动 tag/release/push。
- 目录结构变化时同步更新 `AGENTS.md` 目录结构、版本号索引同步清单与 `.gitignore`。
- 临时测试/脚本/报告放 `.agent/` 或 `temp/`,避免污染项目本体。
- 每次变更新增"变更大纲 + 部分重要细节",不照抄全文。
