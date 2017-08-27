---
title: 科学上网方法总结
date: 2017-08-27 00:38:34
tags: [vps,shadowsocks,科学上网]
categories: [科学上网]
---

# shadowsocks
## 购买vps
Digital Ocean
搬瓦工
## 部署ss服务端
### 安装
shadowsocks python版本安装
shadowsocks-libev安装，参见[shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev)
<!-- more -->
shadowsocks-libev正常配置出错，后卸载使用一键脚本安装，参见[shadowsocks libev 一键安装](https://github.com/iMeiji/shadowsocks_install/wiki/shadowsocks-libev-%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85)，之后也可通过`vim /etc/shadowsocks-libev/config.json`配置服务端

### 卸载方法
使用 root 用户登录，运行以下命令：`./shadowsocks-libev.sh uninstall`

### 使用命令
启动：`/etc/init.d/shadowsocks start`
停止：`/etc/init.d/shadowsocks stop`
重启：`/etc/init.d/shadowsocks restart`
查看状态：`/etc/init.d/shadowsocks status`

## 开启bbr
参见[开启TCP BBR拥塞控制算法](https://github.com/iMeiji/shadowsocks_install/wiki/%E5%BC%80%E5%90%AFTCP-BBR%E6%8B%A5%E5%A1%9E%E6%8E%A7%E5%88%B6%E7%AE%97%E6%B3%95)
## 部署ss客户端
参见[Shadowsocks for Windows](https://github.com/shadowsocks/shadowsocks-windows/releases)
## 配置chrome
Switchyomega可以系统代理，自动切换出错：迅雷的问题，参见[github issue](https://github.com/FelisCatus/SwitchyOmega/issues/557)
## 配置手机版本
影梭
# XX-net
# lantern