# 公开仓迁移说明

## 这次迁移做了什么

本仓按芯片与工程用途整理已经发布的源码。从各项目自己的公开 Tag 导出，不把旧统一仓当前分支误当成每个历史版本的发布源码，也不带入开发工作树的未发布改动。

固件、文件系统镜像和启明随包烧录说明按原文件字节迁移；不重新编译、不重命名固件，也不把它们写进 Git 历史。新的 `release-manifest.json` 与 `SHA256SUMS.txt` 负责记录此次迁移的来源与校验关系，不替换原来的构建事实。

## 本仓来源映射

| 原公开 Tag / Release | 原公开 commit | 原目录 | 本仓目录 |
| --- | --- | --- | --- |
| [g32s10x-minimal-integration-v0.8.3](https://github.com/tangeai/tirtc-device-example/releases/tag/g32s10x-minimal-integration-v0.8.3) | `ad9a41b349adc92b731c6255c709565de57f48a6` | `sdk-integration-examples/g32s10x` | [g32s10x/sdk-integration-examples](../g32s10x/sdk-integration-examples/README.md) |
| [g32s10x-device-monitor-v0.1.1](https://github.com/tangeai/tirtc-device-example/releases/tag/g32s10x-device-monitor-v0.1.1) | `ad9a41b349adc92b731c6255c709565de57f48a6` | `complete-applications/g32s10x/device-monitor` | [g32s10x/complete-applications/device-monitor](../g32s10x/complete-applications/device-monitor/README.md) |

这里的 commit 是原公开仓中的源码身份，和项目 `SOURCE_PROVENANCE.md` 中可能记录的开发来源 commit 不是同一层。两者都保留，不把旧 SHA 改写成新仓 SHA。新仓发布 commit 与 Tag 以对应 Release 的迁移 manifest 为准。

## 怎样核对

1. 在新 Release 的 manifest 中确认操作是迁移，找到原仓库、Tag、commit、项目路径和原 manifest 来源。
2. 按旧来源 SHA 与新路径映射核对源码；文档入口或路径调整属于迁移变更，代码、SDK 与原始来源记录分别核对。
3. 下载同一 Release 的附件，计算 SHA-256，与新的 `SHA256SUMS.txt` 及原资产记录比对。
4. 需要重建时，按工程 README 准备环境并记录新的构建输入与输出；本次迁移没有执行这一步。
5. 把静态核验、原始构建、烧录、联网、媒体与长时间运行分别记录。附件一致只证明文件身份，不新增真机结论。

## 文档与历史

四章开发者指南和公共历史资料源自旧统一仓 `cb183be67f10b4cad7895a5be7a6799e10cc4281`，保留正文与配图，调整导航并注明跨平台案例。原项目 `SOURCE_PROVENANCE.md`、版本和 SDK manifest 维持其历史证据值；其中旧目录、开发路径和 SHA 是来源记录，不是本仓的构建命令。

[旧统一仓](https://github.com/tangeai/tirtc-device-example)保留历史源码、Release 与迁移入口，完成迁移验收后归档，不删除历史下载。新的芯片入口分别是[小钛 ESP32](https://github.com/tangeai/xiaotai-esp32)和[小钛君正](https://github.com/tangeai/xiaotai-ingenic)。

[探鸽 + 启明独立仓](https://github.com/tangeai/tirtc-device-qiming)继续保留独立定位。本次仅把其 `1.0.2` 公开源码和原附件收录到乐鑫仓专用板卡目录，不修改该独立仓，也不套用 Waveshare 的板级配置、音频能力或烧录说明。

## 许可与边界

仓库改名不改变原版权和授权。根 MIT 不覆盖 TiRTC SDK、供应商组件、第三方字体与素材的独立条款，详见[第三方组件与 SDK](THIRD_PARTY_NOTICES_CN.md)。迁移没有移除功能、替换 SDK、升级依赖、生成新的固件或完成新的设备回归。
