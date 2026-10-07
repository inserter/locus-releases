# LOCUS

少记几个地址，少维护一份启动页。

在自己托管的一个页面里，找到并打开 NAS、家庭服务器和自建应用。

[English](README.md) · [官网](https://locus.casa/zh/) · [下载](https://github.com/inserter/locus-releases/releases/latest) · [反馈](https://github.com/inserter/locus-releases/issues)

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://locus.casa/shots/home-zh-dark-1x.webp">
  <img src="https://locus.casa/shots/home-zh-light-1x.webp" alt="LOCUS 家庭实验室范例：固定的应用和网站、最近打开的入口，以及按设备整理的服务。" width="760">
</picture>

*范例环境。设备名称和服务数据用于演示。*

## 家里的设备，家里的服务

一台 NAS 有文件共享、管理后台，也可能有 SSH。家庭服务器跑着几个应用，各占一个端口。有些地址在收藏夹里，有些只剩浏览器历史记录。

LOCUS 把能发现的服务放到一个页面，按设备整理。没有广播的应用自己补上，常用的入口固定下来。不必先写好一整份面板配置，才能开始使用。

## 找到服务，打开所需

- **自动发现：**通过 mDNS、SSDP/UPnP 和 WS-Discovery 查找广播出来的服务。能发现什么，取决于设备广播什么，以及运行 LOCUS 的机器能收到什么。
- **搜索入口：**输入 `nas` 或 `ssh nas` 等几个词缩小结果。网页在新标签页打开。
- **常用的放近一点：**在启动台固定、排序和分组。最近打开的入口方便再次访问；暂时找不到的固定项变灰，不直接消失。
- **补上不广播的应用：**粘贴地址、检查连接，用页面标题预填名称。可以归到一台设备，也可以单独添加外部网站。
- **查看候选：**可选端口探测提出候选入口，由你接受或忽略。候选探测和定时扫描默认关闭。
- **按自己的习惯使用：**中英文界面、明暗模式，以及主题、图标和背景设置。

同一台设备可以有多个入口。LOCUS 不保证自动发现每个自建应用，也不会替你登录目标服务。

## 安装

在可信家庭网络中的兼容 Linux 机器上运行 LOCUS，然后打开 `http://<主机IP>:3033`。

### Linux 原生安装

```sh
curl -fsSL https://locus.casa/releases/install | sh
```

安装器询问语言、检查环境，再引导设置监听地址、端口和数据目录。安装专用的 `locus` 用户以及 systemd 或 OpenRC 服务需要 root 或 sudo 权限。

当前原生发布包支持 **x86_64 和 ARM64（aarch64），要求 glibc 2.34 或更新**。并非所有 Linux NAS 都满足要求。ARMv7 不是 ARM64；目前不能把 musl/Alpine 原生包或 ARMv7 包视为已支持。

手动下载和校验方法见[安装说明](https://locus.casa/zh/#manual)及 [Releases](https://github.com/inserter/locus-releases/releases)。

### Linux 上的 Docker

```sh
curl -fsSL https://locus.casa/releases/latest/docker-linux-amd64.tar.gz | docker load
docker run -d --name locus --network host --restart unless-stopped -v locus-data:/data locus:latest
```

ARM64 机器将下载地址中的 `amd64` 换成 `arm64`。运行命令的用户需要 Docker 权限。发现功能要求 host 网络，不使用端口映射。`locus-data` 卷保存设置和记录。

使用其他端口时，在 `docker run` 命令末尾加上 `--bind 0.0.0.0:<端口>`。NAS 必须已有兼容、可用的 Docker 环境；Docker 不能解决 CPU 架构不匹配。LOCUS 不管理容器。

## 使用前需要知道

- **仅用于可信局域网。**LOCUS 没有内置登录。能访问页面的人可以修改设置和使用可用的设备控制。Host 和同源检查是浏览器防护，不是认证；不要直接暴露到公网。
- **发现有网络边界。**不会自动跨越隔离的 VLAN 或 VPN。服务需要适当的广播、网络转发或手动登记。
- **非网页入口需要客户端。**SSH、SMB、VNC 等链接依赖浏览器和操作系统关联了相应客户端。LOCUS 不内置终端、文件管理器或远程桌面。
- **状态是上次检查，不是保证。**可达性与发现证据分开记录。HTTP 5xx 显示为服务异常，不算健康响应。
- **地址跟随有条件。**稳定身份和当前邻居记录可以帮助符合条件的手动入口跟随 IP 变化。绑定有歧义时需要确认，不等于动态 DNS。
- **主动探测可选。**仅在有权检查的网络开启扫描。它会产生流量，也可能触发安全告警；先从小范围开始。

设备控制范围有限：支持的 UPnP 媒体播放器可提供播放、音量等操作；IPP 打印机和 ONVIF 摄像头提供只读信息和相关链接。支持的控制默认开启。可用 `LOCUS_CONTROL_CLIENTS` 限制控制来源（`none` 禁止所有客户端），或在态势面板逐项关闭。LOCUS 不是通用智能家居控制台或监控系统。

## 数据留在自己的机器上

入口、发现记录、设置、启动台布局、使用历史和上传的资源都保存在数据目录：安装器默认使用 `/var/lib/locus`，Docker 使用 `/data` 卷。

不需要 LOCUS 云账户。本地存储**不等于没有网络请求**：发现、连接检查、标题和图标获取、设备控制都会访问目标。登记外部网站可能产生公网请求。发布版也会检查更新；设置 `LOCUS_UPDATE_CHECK=off` 可关闭更新检查。

升级前停止 LOCUS，备份**整个数据目录**。不要仅换回旧可执行文件来降级。Docker 内的页面升级把新程序写入 `/data`，不会替换 Docker 映像。

部署边界和网络行为的详细说明见[官网](https://locus.casa/zh/#before)。

## 反馈

这个仓库用于官方二进制发布、版本说明和用户反馈。**LOCUS 是闭源软件，这里不是程序源码仓库。**

发现遗漏或不顺手的地方，可以[提交 Issue](https://github.com/inserter/locus-releases/issues)。请注明版本、操作系统和 CPU 架构、安装方式、预期结果和复现步骤。发现问题还请说明网络是否隔离、服务是否开启广播。

截图和日志请先脱敏。不要上传密码、Token、私钥或数据库。

## 许可

可以在自己控制的机器上，免费运行未修改副本，供个人或内部使用。再分发、修改等使用受到 [LOCUS 许可](https://locus.casa/license.txt)限制。第三方组件保留各自许可，见[第三方声明](https://locus.casa/third-party-notices.txt)。Docker 系统组件信息随映像提供，文件名为 `SYSTEM_COMPONENTS.txt`。

LOCUS 的全称是 *Local Object & Capability Unified Space*。
