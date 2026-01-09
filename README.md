# tun2socks 一键安装脚本

[English](README_EN.md) | 简体中文

> Fork from [hkfires/onekey-tun2socks](https://github.com/hkfires/onekey-tun2socks)

一键安装 [hev-socks5-tunnel](https://github.com/heiher/hev-socks5-tunnel)，为 VPS 添加 IPv4 或 IPv6 出口。

## ✨ 新功能

相比原项目，本 fork 新增了以下功能：

- **IPv6 出口模式**：支持为 IPv4 Only / 双栈主机添加 IPv6 出口（例如通过 WARP SOCKS5 代理获取 IPv6）
- **智能 DNS 配置**：仅在需要时启用 DNS64，避免在有正常网络的主机上误改 DNS
- **完善的路由规则**：自动检测并排除主机 IP 和 SOCKS5 代理地址

## 📋 使用场景

| 场景 | 模式选择 | 说明 |
|-----|---------|-----|
| IPv6 Only 主机需要 IPv4 | 添加 IPv4 出口 | 如 Alice Networks 的纯 IPv6 VPS |
| IPv4 Only / 双栈主机需要 IPv6 | 添加 IPv6 出口 | 如使用 WARP SOCKS5 获取 IPv6 |

## 🚀 快速开始

### 安装

```bash
# 使用 Alice Networks 预设
sudo bash install_tun2socks.sh -i alice

# 使用自定义 SOCKS5 代理
sudo bash install_tun2socks.sh -i custom
```

安装时会提示选择：
1. **添加 IPv4 出口** - 适用于 IPv6 Only 主机
2. **添加 IPv6 出口** - 适用于 IPv4 Only / 双栈主机

### 其他命令

```bash
# 卸载
sudo bash install_tun2socks.sh -r

# 切换 Alice 模式端口
sudo bash install_tun2socks.sh -s

# 检查脚本更新
sudo bash install_tun2socks.sh -u

# 查看帮助
bash install_tun2socks.sh -h
```

### 服务管理

```bash
# 查看状态
systemctl status tun2socks.service

# 启动/停止/重启
systemctl start tun2socks.service
systemctl stop tun2socks.service
systemctl restart tun2socks.service

# 查看日志
journalctl -u tun2socks.service -f
```

## 📁 配置文件位置

| 文件 | 路径 |
|-----|-----|
| 服务配置 | `/etc/systemd/system/tun2socks.service` |
| 程序配置 | `/etc/tun2socks/config.yaml` |
| 二进制文件 | `/usr/local/bin/tun2socks` |

## 🏢 支持的服务商

### Alice Networks

[Alice Networks](https://www.alicenetworks.net/) 大善人提供免费的纯 IPv6 VPS，脚本内置了其 SOCKS5 代理节点配置。

使用 `alice` 模式安装，可选择多个台湾家宽出口节点（端口 10001-10008）。

### Legend VPS

⚠️ **注意**：Legend 模式可能已不再适用，因为原作者未维护该服务商的配置。建议使用 `custom` 模式手动配置。

### 自定义模式 (Custom)

使用 `custom` 模式可以配置任意 SOCKS5 代理作为出口。

**示例：将 Cloudflare WARP Proxy 转换为 TUN 网卡**

如果你已经在本机运行了 WARP，并且它监听在 `127.0.0.1:10000`（SOCKS5 代理模式），你可以使用本脚本将其转换为系统级的 TUN 网卡：

```bash
sudo bash install_tun2socks.sh -i custom
```

安装过程中的配置示例：
```
请选择要添加的出口类型
请选择 (1 或 2, 默认为 1): 2  # 选择添加 IPv6 出口

请输入 Socks5 服务器地址: 127.0.0.1
请输入 Socks5 服务器端口: 10000
请输入用户名 (可选): [直接回车]
```

这样配置后，所有 IPv6 流量都会通过 WARP 代理，而 IPv4 流量保持直连。

> **💡 为什么只代理 IPv6？**
> 
> WARP 的 IPv4 地址由于被大量用户共享，IP 信誉度较差，经常被各种网站标记或限制。相比之下，WARP 的 IPv6 地址质量较好。因此建议只使用 WARP 获取 IPv6 连接，保持 IPv4 直连以获得更好的访问体验。

## 📜 许可证

本项目基于 [hkfires/onekey-tun2socks](https://github.com/hkfires/onekey-tun2socks) 修改，感谢原作者的贡献。
