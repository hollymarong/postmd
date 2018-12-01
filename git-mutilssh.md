---
title: git生成多个ssh配置 
date: 2017-02-21 09:01:46
tags: Git
---
ssh是一种网络协议，用于计算机之间的加密登录。
### github 生成ssh key
1. 账户setting/SSH and GPG keys
```
ssh-keygen -t rsa -b 4096 -C "hollymarong@example.com"
//输入两次密码
enter password twice
//打开ssh-agent
eval "$(ssh-agent -s)"
//在ssh-agent中添加SSH key
 ssh-add ~/.ssh/id_rsa
//复制ssh key，并在github账户中添加ssh key
clip < ~/.ssh/id_rsa.pub
```
### gitlab 生成ssh key
1. 账户setting/SSH  Keys
```
ssh-keygen -t rsa -C "marong@corp.netease.com"
//重命名文件~/.ssh/id_rsa_netease_nex，不要覆盖~/.ssh/id_rsa
//输入两次密码
enter password twice
//打开ssh-agent
eval "$(ssh-agent -s)"
//在ssh-agent中添加SSH key
 ssh-add ~/.ssh/id_rsa_netease_nex
//复制ssh key，并在github账户中添加ssh key
clip < ~/.ssh/id_rsa_netease_nex.pub
```
### 在~/.ssh文件夹中，新建config文件
```
//新建config文件
touch config
//配置config文件
#  github user(marong@corp.netease.com)  netease nex
Host gitlab.ws.netease.com
User marong
IdentityFile ~/.ssh/id_rsa_netease_nex

# Default github user(hollymarong@gmail.com)  默认配置，一般可以省略
Host github.com
User hollymarong
Identityfile ~/.ssh/id_rsa
```
### 检测ssh是否配置成功
```
ssh -T git@github.com
//Hi hollymarong! You've successfully authenticated, but GitHub does not provide shell access.
ssh -T git@gitlab.ws.netease.com
//ssh: connect to host gitlab.ws.netease.com port 22: Connection refused,提示连接不上，是权限的问题，不影响具体项目

```
### git bash 客户端的问题
配置后以上之后，如果gitbash 出现error 证书的问题，这时候卸载git bash 重新安装之后就能拉取项目了
