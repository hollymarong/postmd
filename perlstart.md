---
title: Perl入门
date: 2017-04-30 18:23:48
tags: Perl
---
* perl程序文件可以不带扩展名字，也可以plx为扩展名
* 查看perl的版本号
```bash
perl -v
```
* 运行文件
```bash
perl hello.pl
```
* 常用命令
```bash
perldoc CGI //查看CGI模块文档
cpan -a //列出所有已安装的模块
```
* mac os下安装local::lib模块
```
// 先从seach.cpan.org下载local::lib模块
//进入local-lib的安装包
cd local-lib-2.000019/
//默认安装到~/perl5
perl Makefile.PL --bootstrap 
make test && make install
perl5 -Mlocal::lib=$HOME/lib)"' >>~/.zshrc
echo 'eval "$(perl -I$HOME/perl5/lib/perl5 -Mlocal::lib)"' >>~/.zshrc
//重启shell
```
* 使用模块
```perl
my $name = "/usr/local/bin/perl/marong";
my $basename = basename $name;
my $dirname = dirname $name;
print "\$basename:$basename"; //marong
print "\$dirname:$dirname"; //'/usr/local/bin/perl'

```
