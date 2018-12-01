---
title: iterm-commandline
date: 2017-06-12 18:44:51
tags:
---
open index.html : 在 浏览器中打开页面
d 列出在会话访问的目录列表，输入列表前的序号，即可直接跳转


## 快捷键
```
Command+R 清空命令行
```

## oh-my-zsh 插件
在.zshrc文件中配置要加载的插件
```
brew install autojump
plugins=(git autojump)
```

### autojump
快速打开文件
在.zshrc,.bash_profile中添加
```
[ -f /usr/local/etc/profile.d/autojump.sh ] && . /usr/local/etc/profile.d/autojump.sh
```
* 用法
安装autojump之后，zsh会自动记录访问过的目录，目录名支持模糊匹配
```
j <目录名>
j --stat 查看历史路径库
```
