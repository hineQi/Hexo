---
title: Mac运行提示已损坏解决办法
tags:
  - Mac
  - 终端
categories:
  - - 参考文件
    - Mac
date: 2020-02-10 23:19:11
---


> 删除
```
删除空目录
rmdir 目录
删除目录及下面所有文件
rm -rf 目录名字
删除文件
rm -f 文件名
```
> 拷贝
```
把驱动目录下的所有文件备份到桌面backup
cp -R /System/Library/Extensions/* /User/用户名/Desktop/backup
```
> 移动/修改名字
```
mv /System/Library/Extensions/AppleHDA.kext /System/Library/Extensions/backup
```
> 删除当前命令行快捷键
```
Ctrl + u
```
> 终端清空所有命令
```
command + K
```
> 终端清除到上一个命令
```
command + L
```
> 查看本机IP
```
ifconfig | grep "inet"
```