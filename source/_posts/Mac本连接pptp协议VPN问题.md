---
title: Mac本连接pptp协议VPN问题
tags:
  - pptp
  - VPN
  - MAC
categories:
  - - 参考文件
    - Mac
date: 2020-02-11 16:00:47
---

# Mac连接pptp协议VPN解决办法

系统偏好设置 -> 网络 -> “+” 。
接⼝选择”VPN”，VPN类型选择”IPSec上的L2TP”，填写服务名称（任意）。
对刚创建的VPN进⾏配置，根据实际vpn地址填写服务地址、账户名称。
鉴定设置… -> 用户鉴定中，勾选密码，填写vpn密码，保存关闭。
创建文件/etc/ppp/options,并进⾏编辑，vim /etc/ppp/options输入如下内容并保存：
plugin L2TP.ppp
l2tpnoipsec
此时连接VPN即可正常访问。