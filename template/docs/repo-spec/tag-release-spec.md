# Tag&Release 规范

## 版本号

遵守 `v<主版本号>.<次版本号>.<修订版本号>` 格式，例如 `v0.1.0` 。

> README的版本徽章显示文字中，版本号不带v字母前缀

## Tag

遵守 `<版本号>` 格式。

## Release

遵守 `<版本号>-dsh-<最佳适用的dsh版本号>` 格式，例如 `v0.1.0-dsh-0.2.1-rc.1` 。

> 本插件的Release绑定于开发插件时开发者使用的dsh版本(可能不是最新版)，并且同一个Release内不做多dsh版本适配。这限制了一个Release只能绑定一个dsh版本号，开发者想要更新dsh版本号就必须迭代版本号并发布新的Release。

## dsh 版本兼容

- 插件版本默认支持的 dsh 范围: `>=0.2.1-rc.1 <=0.2.2-rc.1`
- 该范围在组合包的 `package.json` 中以 `engines.dsh` 声明
- 范围的依据与文档收录: dsh 0.2.1-rc.1 至 0.2.1-rc.5 之间 `docs/user/develop/` 无变更,对应收录目录 `docs/dsh-dev-docs/dsh-0.2.1-rc.1/`;dsh 0.2.2-alpha.2 至 0.2.2-rc.1 之间亦无变更,相对 0.2.1-rc.5 仅两处文字更新,对应收录目录 `docs/dsh-dev-docs/dsh-0.2.2-rc.1/`;范围上界取已核对的最新版本
- 默认范围收窄或扩展时,同步更新官方文档收录目录、`engines.dsh` 与本段落
- 0.2.2 后续预发布或正式版本发布后,复核 `docs/user/develop/` 是否变化,必要时新增收录目录并同步本段落与 `engines.dsh` 范围
