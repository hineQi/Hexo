---
title: Mac运行提示已损坏解决办法
tags:
  - Mac
  - 已损坏
categories:
  - - 学习
    - Mac
date: 2020-02-10 23:19:11
---

1.开启安全隐私中的任何来源
```
sudo spctl --master-disable
```
2.上面那步还不可以就执行下面这句
```
sudo xattr -r -d com.apple.quarantine /Applications/WebStorm.app
```