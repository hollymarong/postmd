---
title: git-commandline
date: 2016-06-09 11:38:18
tags: Git
---

# Git操作

* git init ： 初始化仓库
* git log: 查看提交日子

## git别名配置
```
git config --global alias.co checkout 
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.unstage 'reset HEAD --'
git unstage fileA
git reset HEAD fileA
git config --global alias.last 'log -1 HEAD'
git last
```
## git clone远程分支

    git clone 地址 
    //如果发现只有一个.git目录，需要checkout了，查看branch信息
    git branch -r  
    //origin/andrio
    //origin/iso
    //origin/pc
    //如果此时我们需要时andriod分支的代码，那么就要进行checkout了

    git checkout origin/andriod

## git  拉取远程分支到本地
git checkout -b localName origin/remoteName

## git 提交远程分支
    git push origin 远程分支的名字
## git 新建分支
git branch test
## git 切换分支
## git 新建并切换到分支
git checkout -b test
## git 更新某个分支
    git fetch [主机名] [分支名]
    git fetch origin master
## git 删除某个分支
    git branch -d [分支名]
    
    git branch -d dev
## git 合并分支
     git merge [分支名]: git merge andrio

## git 提交
    git add -A || git add -all
    git commit -am 'first commit'
## git不上传空文件夹设置
```
//方法1
find . -type d -empty -exec touch {}/.gitignore \;
```

## git push
```
git push origin <branch> //提交远程分支
git push origin :<branch> //删除远程分支
```

## git branch 
```
git branch // 查看所有分支
git branch -r// 查看远程分支
git branch -d <branch> //删除本地分支
git checkout -b <branch> origin/<branch>
git branch -m <newbranch name> //修改当前分支名为newbranch name
git branch -m <old branch> <new branch> //修改old-branch 为 new-branch
```

## git pull
```
git pull 
git pull origin <branch> //更新分支代码
```

## git checkout
``` 
git checkout master //切换到master分支
git checkout <file> //将暂存区的文件覆盖工作区的文件
git checkout <commit> <file> //将工作目录中的替换成commit中的file文件，并加入到缓存区
git checkout <commit>
git checkout -p <branch> //对比当前分支和branch分支的区别，并选择是否将这些差异应用到当前分支上

git checkout -b <new branch name> <commit id> //根据特定的commit创建分支并合并
git checkout -b <branch> origin/<branch> //拉取全程分支到本地
//等同于下面两个命令
git checkout <commit id>
git checkout -b <new branch name>


git checkout -- . //放弃本地修改
git clean -df  //删除新增文件

```
## git merge
```
//将master分支合并到feature分支最简单的办法就是用下面这些命令：
git checkout feature
git merge master
//或者，你也可以把它们压缩在一行里。
git merge master feature

```
## git rebase
```
git checkout new-feature
git rebase -i master

git checkout master
git merge new-feature

```

## git revert
撤销
## git reset
重做
```
git reset --hard <commit id> //reset到指定的commit
git reset --hard HEAD^ // 回退到上一个commit 
```

##  git log
```
git log andrio   //查看某个分支的log
git log <branch> --  //查看分支log
git log -- <directory>  //查看directory文件夹的log
git log master..test  //查看test分支的提交记录但不包含master分支记录
git log test..master //查看master分支记录但不包含test分支记录
git log test...master //查看test和master分支记录
git log test --not master //屏蔽master分支log
git log <path> // 查看某些文件或目录的历史提交
git log -p  //查看每次内容提交的差异
git log -p [filename] //查看文件的log
git log -p -2 //查看最近两次提交的内容差异
git log -stat -1 //查看最近一次提交的文件增删数量
git log --author marong02 // 查看提价者的日志
git log --grep=test //通过提交说明信息包含test字段过滤提交日志
git log --pretty=oneline // 将提交日志压缩到一行
git show commitId // 展示某次commit的详情
```
## git stash
```
git stash 
git stash list 
git stash apply  重新应用刚刚的存储
git stash apply stash@{2} 重新应用更早的存储
```
## git clean
从工作目录中移除未跟踪的文件
```
git clean -df //删除未跟踪的文件，但不删除.gitignore中忽略的文件
```
## git diff -w [filename]： 查看文件的合并情况
## 如果希望保留生产服务器上所做的改动,仅仅并入新配置项, 处理方法如下:

git stash
git pull
git stash pop

## 如果希望用代码库中的文件完全覆盖本地工作版本. 方法如下:

git reset --hard
git pull

#Setting your branch to exactly match the remote branch can be done in two steps:
git fetch origin
git reset --hard origin/master
## git放弃某个文件的修改
git checkout <filename>

## 合并多个commmit为一个,不好看
git rebase -i 允许合并除了root commit的多条提交日志
git rebase -i --root 允许合并包含root commit的多条提交日志
在提交PR之前，用rebase合并提交，PR提交后，任何来自其他开发者的更改要有merge，
如果用base会造成Git和其他开发者难以找到这个分支接下来的任何提交
```
//方案一
git rebase -i <commit id> //要合并的版本之前的版本号，输入的版本不参与合并
//在交互式窗口中，将第二行开始的pick，手动改成s或f，改完后wq保存退出
//如果出现冲突，需要解决，解决完成后，git add . => git rebase --continue
//成功后,如果想放弃本次修改，可以git rebase --abort
git push --force origin master //提交到远程分支

//方案二
git rebase -i @~4
修改下面的三个中开头的pick为f,然后保存,合并多个commit为一个
git push origin --delete feature/FECHI-4927 //删除分支
git push origin feature/FECHI-4927 //重新提交分支
```

## git提PR步骤
合并好commit之后
进入branch --> create pull request --> 
输入reviewers,在控制栏里执行

```
document.querySelector('.select2-offscreen').value="duzhaoyang|!|zhengwenbing|!|zhangcongjie|!|chenwenbin03|!|huxiaoyue|!|wangzengdi|!|fuhongjuan|!|chengang07|!|wangyue08|!|chenzeqing|!|zhangchunxiao|!|chenshenqian|!|shiwenzhe"
```
--> Create

### git 合并master
git merge origin/master //merge 不需要重新提PR git pull --rebase origin master //rebase 需要重新提 PR

## git cat-file
## git stash 
## git reflog 能召回git reset的操作，，如果垃圾回收了git reflog，就永远找不回了

### 多人在一个分支开发
```
git pull 
git rebase
```

### git grep "" 搜素内容

### git commit 
```
git commit --amend --date=now 修改日期
git commit --amend --no-edit 直接提交
```

### Tig 使用
```

tig r  // 查看分支详情


tig log [filename] // 查看某个文件的log


q // 退出某次view 

tig --author=[name] // 查看author提交记录

/[filter]  // 对view搜索

Ctrl + c   // 退出对view的搜索

tig 进入diff 之后
e  //  对文件进行编辑


```
