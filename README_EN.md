# tun2socks One-Click Installation Script

English | [简体中文](README.md)

> Fork from [hkfires/onekey-tun2socks](https://github.com/hkfires/onekey-tun2socks)

One-click installation of [hev-socks5-tunnel](https://github.com/heiher/hev-socks5-tunnel) to add IPv4 or IPv6 exit for your VPS.

## ✨ New Features

Compared to the original project, this fork adds:

- **IPv6 Exit Mode**: Support adding IPv6 exit for IPv4 Only / Dual-stack hosts (e.g., via WARP SOCKS5 proxy)
- **Smart DNS Configuration**: Only enables DNS64 when needed, avoiding unnecessary DNS changes on hosts with normal connectivity
- **Complete Routing Rules**: Automatically detects and excludes host IP and SOCKS5 proxy addresses

## 📋 Use Cases

| Scenario | Mode | Description |
|----------|------|-------------|
| IPv6 Only host needs IPv4 | Add IPv4 Exit | e.g., Alice Networks pure IPv6 VPS |
| IPv4 Only / Dual-stack needs IPv6 | Add IPv6 Exit | e.g., using WARP SOCKS5 to get IPv6 |

## 🚀 Quick Start

### Installation

```bash
# Use Alice Networks preset
sudo bash install_tun2socks.sh -i alice

# Use custom SOCKS5 proxy
sudo bash install_tun2socks.sh -i custom
```

During installation, you'll be prompted to choose:
1. **Add IPv4 Exit** - For IPv6 Only hosts
2. **Add IPv6 Exit** - For IPv4 Only / Dual-stack hosts

### Other Commands

```bash
# Uninstall
sudo bash install_tun2socks.sh -r

# Switch Alice mode port
sudo bash install_tun2socks.sh -s

# Check for script updates
sudo bash install_tun2socks.sh -u

# Show help
bash install_tun2socks.sh -h
```

### Service Management

```bash
# Check status
systemctl status tun2socks.service

# Start/Stop/Restart
systemctl start tun2socks.service
systemctl stop tun2socks.service
systemctl restart tun2socks.service

# View logs
journalctl -u tun2socks.service -f
```

## 📁 Configuration File Locations

| File | Path |
|------|------|
| Service config | `/etc/systemd/system/tun2socks.service` |
| Program config | `/etc/tun2socks/config.yaml` |
| Binary | `/usr/local/bin/tun2socks` |

## 🏢 Supported Providers

### Alice Networks

[Alice Networks](https://www.alicenetworks.net/) offers free pure IPv6 VPS. The script includes built-in SOCKS5 proxy node configurations.

Use `alice` mode to install, with multiple Taiwan residential exit nodes available (ports 10001-10008).

### Legend VPS

⚠️ **Note**: Legend mode may no longer work as the original author has not maintained the configuration for this provider. It's recommended to use `custom` mode for manual configuration.

### Custom Mode

Use `custom` mode to configure any SOCKS5 proxy as your exit.

**Example: Convert Cloudflare WARP Proxy to TUN Interface**

If you're already running WARP locally and it's listening on `127.0.0.1:10000` (SOCKS5 proxy mode), you can use this script to convert it to a system-level TUN interface:

```bash
sudo bash install_tun2socks.sh -i custom
```

Configuration example during installation:
```
Please select the exit type to add
Please choose (1 or 2, default is 1): 2  # Choose to add IPv6 exit

Please enter Socks5 server address: 127.0.0.1
Please enter Socks5 server port: 10000
Please enter username (optional): [Press Enter]
```

With this configuration, all IPv6 traffic will go through the WARP proxy, while IPv4 traffic remains direct.

> **💡 Why only proxy IPv6?**
> 
> WARP's IPv4 addresses are shared among many users, resulting in poor IP reputation and frequent blocking or restrictions by various websites. In contrast, WARP's IPv6 addresses have better quality. Therefore, it's recommended to only use WARP for IPv6 connectivity while keeping IPv4 direct for a better access experience.

## 📜 License

This project is modified from [hkfires/onekey-tun2socks](https://github.com/hkfires/onekey-tun2socks). Thanks to the original author for their contribution.
