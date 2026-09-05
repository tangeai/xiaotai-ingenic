# 固件下载与源码获取

## 选择项目 Release

[小钛君正 Releases](https://github.com/tangeai/xiaotai-ingenic/releases)按项目分发固件。本仓尚未提供 G32S10X 最小系统源码或 Release，以下两个项目各自独立。

| 项目 | 下载入口 | 必需固件 |
| --- | --- | --- |
| G32S10X 最小集成 | [0.8.3](https://github.com/tangeai/xiaotai-ingenic/releases/tag/g32s10x-minimal-integration-v0.8.3) | `g32s10x-tirtc-minimal-rtos-with-spl-v0.8.3.bin` |
| G32S10X Device Monitor | [0.1.1](https://github.com/tangeai/xiaotai-ingenic/releases/tag/g32s10x-device-monitor-v0.1.1) | `g32s10x-tirtc-device-monitor-rtos-with-spl-v0.1.1.bin`、`g32s10x-tirtc-device-monitor-fs-v0.1.1.yaffs2`、`g32s10x-tirtc-device-monitor-data-v0.1.1.yaffs2` |

每次同时下载同一 Release 的 `SHA256SUMS.txt` 和 `release-manifest.json`。完整应用的主固件、`fs`、`data` 不跨版本混用。供应商 SDK、交叉工具链和 Cloner 不随仓库分发，需要从授权渠道获取。

## 校验与烧录

Windows PowerShell：

```powershell
Get-FileHash .\downloaded-firmware.bin -Algorithm SHA256
```

Linux：

```bash
sha256sum ./downloaded-firmware.bin
```

macOS：

```bash
shasum -a 256 ./downloaded-firmware.bin
```

文件名、大小与 SHA-256 一致后，按[最小集成 README](../g32s10x/sdk-integration-examples/README.md)或[完整应用 README](../g32s10x/complete-applications/device-monitor/README.md)，使用君正 Cloner 与目标板 policy 烧录。不要把 ESP32 的网页烧录或 `0x0` 地址套用到 G32S10X。

本次是公开源码与原版本固件的迁移，**没有从新仓重新构建固件**。新 `release-manifest.json` 明确记录迁移操作、旧来源和新路径；原始 manifest 的来源链接与 SHA-256 保留，新的 `SHA256SUMS.txt` 覆盖目标 Release 附件。原固件字节不变，原始构建与目标板验证范围也不变。

G32S10X Device Monitor `0.1.1` 的原始主固件构建与文件系统打包具有不同 Git tree 的历史边界，不能仅凭迁移后的文件一致就说它们来自同一个新仓构建。详见[版本与证据](VERSIONS_CN.md)及[迁移说明](MIGRATION_CN.md)。烧录后仍需分别核对启动、联网、绑定、媒体与重复操作。

## 按项目获取源码

GitHub 的 **Download ZIP** 和 Release 自动生成的 **Source code** 包是整个仓库快照。只需要一个工程时，可使用支持 partial clone 和 sparse-checkout 的 Git：

```bash
git clone --depth 1 --filter=blob:none --sparse --branch g32s10x-minimal-integration-v0.8.3 https://github.com/tangeai/xiaotai-ingenic.git
cd xiaotai-ingenic
git sparse-checkout set g32s10x/sdk-integration-examples docs
cd g32s10x/sdk-integration-examples
```

换工程时同时替换 Tag 和路径：

| 项目 Tag | 完整项目目录 |
| --- | --- |
| `g32s10x-minimal-integration-v0.8.3` | `g32s10x/sdk-integration-examples` |
| `g32s10x-device-monitor-v0.1.1` | `g32s10x/complete-applications/device-monitor` |

保留整个项目目录，不能只下载应用源码。`port`、`integration`、`sdk`、测试素材和工具都是项目接入的一部分。`--sparse` 默认的 cone 模式还会保留根目录与父级文件；显式加入 `docs`，保留四章、配图与下载说明。随后按项目 README 把覆盖层接入匹配的外部供应商 SDK，稀疏检出不替你安装工具链。

`--depth 1` 限制历史，`--filter=blob:none` 在服务端支持时按需获取文件内容，sparse-checkout 控制检出范围。后续切换项目或读取其他历史可能再次下载；这里不承诺固定体积。Tag 检出后处于 detached HEAD，修改代码前可另建工作分支。

已按公开源码静态检查本项目没有需要一并检出的兄弟工程依赖；本次没有执行稀疏检出构建、烧录或真机验证。S3/P4 的构建与固件入口见[小钛 ESP32 下载说明](https://github.com/tangeai/xiaotai-esp32/blob/main/docs/RELEASES_CN.md)。
