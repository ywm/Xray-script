# Xray-TLS+Web 搭建/管理脚本

面向个人 VPS 的 Xray 与 Web 搭建、更新和管理脚本。

## 脚本特性

- 默认搭建 VLESS-REALITY、Trojan-TLS、VLESS-XHTTP 三种协议，由 Nginx 前置分流。
- 使用 acme.sh 申请和续期泛域名证书，支持 XHTTP CDN 接入与上下行分离。
- 支持 Cloudreve、Nextcloud、403 页面、自定义静态网站和反向代理网站。
- 提供分享链接、Base64 订阅和 Mihomo（Clash Meta）YAML 配置。
- 支持 Xray 多文件配置，可查看、验证和备份；安装/更新时保留额外添加的自定义 JSON 文件。
- 集成组件更新、BBR/锐速安装及系统精简等管理功能。

## 注意事项

- 使用 root 用户运行，建议在纯净系统上安装。
- 需要将域名解析到服务器，证书申请按提示配置 Cloudflare DNS 凭据。
- 适合个人使用，不提供多用户管理和流量统计。
- Nginx 和可选的 PHP 使用源码编译，安装耗时取决于服务器性能及网络。
- Cloudreve 不需要 PHP；Nextcloud 需要 PHP，占用的资源更多。
- 请妥善保管密钥、分享链接和订阅地址。

## 脚本使用说明

### 1. 安装依赖

Debian / Ubuntu：

```bash
apt --no-install-recommends -y install wget ca-certificates || (apt update && apt --no-install-recommends -y install wget ca-certificates)
```

Red Hat 系：

```bash
dnf -y install wget ca-certificates || yum -y install wget ca-certificates
```

### 2. 获取/更新脚本

```bash
wget -O Xray-TLS+Web-setup.sh https://github.com/ywm/Xray-script/raw/main/Xray-TLS+Web-setup.sh
```

### 3. 执行脚本

```bash
bash Xray-TLS+Web-setup.sh
```

按菜单提示操作：`1` 安装/重新安装，`2` 整体更新，`9` 单独更新 Xray，`15` 查看配置和订阅，`22` 管理多文件配置。重新安装与整体更新不同，已有部署请选择对应功能。

## 自定义配置

Xray 配置目录为 `/usr/local/etc/xray/config.d/`。脚本管理 `00_`、`10_`、`20_`、`21_`、`22_`、`50_`、`60_`、`69_` 开头的 JSON 文件，重新生成配置时会覆盖这些文件。

额外配置请使用其他编号，例如 `51_routing.json`、`61_wireguard.json`。整体安装/更新会自动备份并恢复这些文件，不再需要时请自行删除。

## 安装位置

| 组件 | 路径 |
| --- | --- |
| Nginx | `/usr/local/nginx` |
| PHP | `/usr/local/php` |
| Cloudreve | `/usr/local/cloudreve` |
| Xray | `/usr/local/bin/xray` |
| Xray 配置 | `/usr/local/etc/xray/config.d/` |

## 相关项目

- 原项目：[kirin10000/Xray-script](https://github.com/kirin10000/Xray-script)
- [Xray-core](https://github.com/XTLS/Xray-core) / [Xray-install](https://github.com/XTLS/Xray-install)
- [acme.sh](https://github.com/acmesh-official/acme.sh)
- [Cloudreve](https://docs.cloudreve.org/) / [Nextcloud](https://docs.nextcloud.com/server/latest/admin_manual/installation/source_installation.html)
- BBR 相关脚本：[teddysun/across](https://github.com/teddysun/across/blob/master/bbr.sh) / [chiakge/Linux-NetSpeed](https://github.com/chiakge/Linux-NetSpeed)
