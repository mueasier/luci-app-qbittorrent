# LuCI for qBittorrent Enhanced Edition

适用于 ImmortalWrt 的 `qbittorrent-enhanced-edition`，依赖由软件包管理器自动安装。
界面使用增强版服务的 UCI 配置：`enabled`、`http_port`、`download_dir`；
服务名和配置文件仍为 `qbittorrent`，程序配置由服务保存在 `/etc/qBittorrent/`。

## GitHub Actions 编译

工作流：`.github/workflows/build.yml`。

- ImmortalWrt：**25.12.1**
- TARGET：**mediatek**
- SUBTARGET：**filogic**
- 设备：**GL.iNet GL-MT2500**，软件包架构为 `aarch64_cortex-a53`

推送 LuCI 或工作流的改动、提交 Pull Request 时自动编译；也可以在 GitHub 的
**Actions → Build for ImmortalWrt 25.12.1 → Run workflow** 手动触发。
完成后在该次运行的 **Artifacts** 下载软件包压缩包。

工作流下载并校验官方 SDK，使用 25.12.1 发布时的 feeds 提交，编译本仓库的
LuCI、简体中文翻译及其依赖（包括增强版后端），保存编译配置、校验和及日志。
这是软件包编译，不生成可刷写固件；GL-MT2500 的不同 PHY 版本使用相同的软件包架构。

仅将本仓库的 `luci-app-qbittorrent/` 放入 SDK。本仓库其余 `qbittorrent/`、
`qt6base/`、`qt6tools/`、`rblibtorrent/` 目录属于原版后端的构建文件，
该工作流使用 ImmortalWrt feeds 中的增强版及依赖。

## 安装

在运行 **ImmortalWrt 25.12.1** 的 GL-MT2500 上，将下载的 LuCI 和中文翻译
`.apk` 上传到 `/tmp`，通过 SSH 执行：

```sh
apk update
apk add qbittorrent-enhanced-edition
apk add --allow-untrusted /tmp/luci-app-qbittorrent-*.apk /tmp/luci-i18n-qbittorrent-zh-cn-*.apk
```

`--allow-untrusted` 用于安装本工作流生成的未签名本地包。
增强版与原版 `qbittorrent` 冲突；如已安装原版，请先备份配置并卸载原版。
安装后在 LuCI 的 qBittorrent 页面设置下载目录、WebUI 端口并启用服务。

## 在 ImmortalWrt 源码中编译

在已更新并安装 feeds 的源码根目录执行（仓库克隆到源码树外，避免扫描到原版后端和 Qt 包）：

```sh
git clone https://github.com/mueasier/luci-app-qbittorrent.git ../qbittorrent-luci-source
rm -f package/feeds/luci/luci-app-qbittorrent
cp -a ../qbittorrent-luci-source/luci-app-qbittorrent package/luci-app-qbittorrent
```

在 `make menuconfig` 中选择 **LuCI → Applications → luci-app-qbittorrent**，然后运行：

```sh
make package/luci-app-qbittorrent/compile V=s
```

参考：[25.12.1 官方 SDK 与目标文件](https://downloads.immortalwrt.org/releases/25.12.1/targets/mediatek/filogic/)、
[增强版软件包与服务定义](https://github.com/immortalwrt/packages/tree/e93a938c63124832d549c41da3157e2ec40fbe05/net/qBittorrent-Enhanced-Edition)。
