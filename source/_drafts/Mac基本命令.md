---
title: Mac基本命令
tags:
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
> 删除当前命令行
```
Ctrl + u
```