---
title: svn-commandline
date: 2016-08-08 10:35:39
tags: svn
---
### svn命令行
- svn help ： 查看svn命令
- svn add
- svn blame
- svn cat
- svn changelist
- svn checkout <co>
- svn commit <ci>
- svn copy
- svn delete
- svn diff <di>
- svn export
- svn import
- svn info
- svn list
- svn lock
- svn log
- svn merge
- svn mergeinfo 
- svn mkdir
- svn move < mv, rename, ren>
- svn patch
- svn propdel <pdel, pd>
- svn propedit <pedit, pe>
- svn propget <pget, pg>
- svn proplist <plist, pl>
- svn propset <pset, ps>
- svn relocate
- svn resolve
- svn resolved
- svn revert
- svn status <stat, st>
- svn switch <sw>
- svn unlock
- svn update <up>
- svn upgrade
### 放弃本地所有修改
```bash
svn revert -R .
svn up
svn revert index.html //恢复某个文件
svn revert modules/tab/tab.css modules/head/head.css ... //恢复多个文件
svn revert module/tabs/ --depth==infinity   //恢复某个文件夹
```
### svn显示最近几条log
```
svn --limit <n> // 显示最近n条log 
svn -l <n> // 显示最近n条log 
svn -l 4 // 显示最近4条log
```
### svn 提交多个文件
- 方法一：
    - svn commit –m”log msg” mydir/dir1/file1.c mydir/dir2/myfile1.h mydir/dir3/myfile3.
- 方法二：
    - $ svn changelist my-changelist mydir/dir1/file1.c mydir/dir2/myfile1.h
    $ svn changelist my-changelist mydir/dir3/myfile3.c etc.
    ... (add all the files you want to commit together at your own rate)
    - $ svn commit -m"log msg" --changelist my-changelist
