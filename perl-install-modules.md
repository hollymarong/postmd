---
title: Perl安装模块
date: 2017-07-17 16:28:21
tags:
---
## 利用cpan安装模块
配置CPAN.pm

```
cpan
o conf makepl_arg INSTALL_BASE=/Users/marong
o conf commit //提交修改
o conf mbuild_arg --install_base /Users/marong
o conf commit //提交修改
o conf //查修配置参数是否生效

cpan IPC::System::Simple //安装模块，安装目录/User/marong/lib/perl5
perldoc IPC::System::Simple //查看安装模块文档
```
