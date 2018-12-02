---
title: mac-commandline
date: 2017-05-10 18:17:26
tags: mac 
---
## mac os
| keybinding           | operate                |
| command+Q            | 退出当前应用           |
| command+M            | 最小化当前应用         |
| command+T            | 切换当前窗口打开的应用 |
| command+H            | 快速隐藏当前应用       |
| command+Shift+Delete | 清除废纸篓             |
| Control + A          | 移动到行首             |
| Control + E          | 移动到行尾             |
| Control + B          | 向前移动一个字符       |
| Control + F          | 向后移动一个字符       |
## mac command-line
which tern //查看安装路径
top //显示所有进程
kill -9 pid //结束pid进程
## curl
```bash
curl -O http://.../.tar.gizp //下载文件到当前目录
```
## terminal
```
cp -r fromPath/file destinationPath
cp ~/test1/*.*  ~/test2/
```
echo 'eval "$(perl -I$HOME/lib/lib/perl5 -Mlocal::lib=$HOME/lib)"' >>~/.bashrc
echo 'eval "$(perl -I$HOME/lib/lib/perl5 -Mlocal::lib=$HOME/lib)"' >>~/.zshrc

## chrome
| keybinding          | operate                  |
| command + Shift + T | 重新打开之前关闭的标签页 |
| command+ Option+I   | devtools                 |
| command + ,         | chrome 设置              |
| command + L         | 快速选中地址栏                  |
      
## alfred
* 安装github

下载workflow工作流，双击安装工作流，自动添加的alfred
生成一个token
gh-auth <your token>

## 登录远程服务器
```
ssh root@47.91.156.125
```
## 查看本机IP
ifconfig

## 查看端口号占用
```
lsof -i :8080
kill -8 pid // 杀掉进程
```
