# 小钛君正

这里是小钛在君正 G32S10X 上的设备端工程。先用最小示例了解 TiRTC 调用链，需要 UI 和完整设备业务时，再进入 Device Monitor。

## 选择工程

| 芯片 | 最小 TiRTC 集成 | 最小系统例子 | 完整应用 |
| --- | --- | --- | --- |
| G32S10X | [SDK 接入 `0.8.3`](g32s10x/sdk-integration-examples/README.md) | **尚未提供源码**，不对应可构建工程或 Release | [Device Monitor `0.1.1`](g32s10x/complete-applications/device-monitor/README.md) |

最小集成聚焦网络、SDK 生命周期和预录媒体发送；完整应用包含君正平台适配、界面、设备绑定与业务流程。供应商 SDK、交叉工具链和 Cloner 需要从授权渠道单独获取，具体版本与覆盖层接入步骤见项目 README。

乐鑫项目的当前支持板型、固件和烧录说明统一从[小钛项目主页](https://github.com/tangeai/xiaotai)进入。四章开发者指南继续保留在本仓；其中的 S3/P4 实现是跨平台参考，不代表君正外设或媒体能力已经验证。

## 先体验，还是先改代码

体验固件时，从[固件下载与校验](docs/RELEASES_CN.md)选择目标项目 Release。完整应用需要配套的主固件、`fs` 和 `data` 镜像，按项目说明与目标板 Cloner policy 烧录，不能套用 ESP32 的 `0x0` 网页烧录步骤。

只开发一个项目时，可以使用支持 partial clone 和 sparse-checkout 的 Git：

```bash
git clone --depth 1 --filter=blob:none --sparse --branch g32s10x-minimal-integration-v0.8.3 https://github.com/tangeai/xiaotai-ingenic.git
cd xiaotai-ingenic
git sparse-checkout set g32s10x/sdk-integration-examples docs
cd g32s10x/sdk-integration-examples
```

取回整个项目目录后，`port`、`integration`、`sdk`、`assets` 和项目工具都会保留；cone 模式也保留仓库根目录及父级说明文件。外部供应商 SDK 和工具链仍需另外准备。Tag 与其他项目路径见[按项目获取源码](docs/RELEASES_CN.md#按项目获取源码)。

GitHub 的 **Download ZIP** 与 Release 自动生成的 **Source code** 压缩包是整个仓库快照，不是当前子目录专用包。浅克隆减少历史，sparse-checkout 控制工作区范围，`--filter=blob:none` 在服务端支持时按需获取文件；不承诺固定下载大小。

## 文档

- [TiRTC 开发者指南：四章连续阅读](docs/README_CN.md#tirtc-开发者指南)
- [固件下载、校验与单项目源码](docs/RELEASES_CN.md)
- [版本与验证边界](docs/VERSIONS_CN.md)
- [迁移来源与新旧目录映射](docs/MIGRATION_CN.md)
- [变更记录](docs/CHANGELOG_CN.md)
- [第三方组件与 SDK 许可范围](docs/THIRD_PARTY_NOTICES_CN.md)

## 版本与来源

本仓保留已公开的项目版本和原固件字节。本次只迁移项目 Tag 源码、目录与下载入口，**不重新编译固件，也不新增烧录、联网或真机验证结论**。新 Release 的 `release-manifest.json` 记录旧来源 SHA 与新路径，固件通过 `SHA256SUMS.txt` 核验。

旧统一仓的源码历史保留，历史固件 Release 和 Tag 已停止提供。当前项目入口统一为[小钛项目主页](https://github.com/tangeai/xiaotai)。各项目 `SOURCE_PROVENANCE.md` 和原始 SDK manifest 继续记录当时的来源；其中的历史路径不能当作新仓路径使用。

## 许可

[根目录 MIT License](LICENSE) 仅覆盖相应权利人提供的原创代码、文档与自有配图。TiRTC SDK、君正供应商源码、第三方库、字体与媒体保留各自条款，仓库迁移不为它们重新授权。使用或再分发前请阅读[第三方组件与 SDK](docs/THIRD_PARTY_NOTICES_CN.md)以及项目内的 LICENSE / NOTICE。
