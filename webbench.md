---
title: webbench
date: 2018-05-03 15:36:11
tags: Nodejs
---
* mac os 安装webbench用于压测
1. 从官网下载压缩包，http://home.tiscali.cz/~cz210552/webbench.html
2. 安装步骤
```
tar -zxvf webbench-1.5.tar.gz // 解压
cd webbench-1.5 // 进入文件夹
sudo mkdir -pv /usr/local/man/man1 // 新建文件夹，以供安装编译时使用
sudo make && sudo make install // 编译安装
```
3. 命令行参数含义

