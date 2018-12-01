---
title: 阿里云服务器centos配置
date: 2017-02-23 08:49:50
tags: Server
---

## 购买ECS实例（申请香港vpc(专有网络服务器带固定公网IP,在服务器内部通过ifconfig查看不到外网网卡eth1)）
## 登录ECS实例
2.1登录方法：
2.1.1.通过阿里远程终端登录
管理实例-远程连接-输入远程终端密码-如果屏幕卡住，按任意键-->输入用户名root，密码-->成功链接到远程实例
2.1.2 通过PuTTY链接linux系统
输入公网ip->输入用户名、密码
##.格式化和挂载数据盘（没有购买数据盘，暂时使用系统盘）
df -h :查看磁盘容量的使用情况
fdisk -l : 查看当前磁盘的情况

## vpn服务器创建步骤
### 先看看你的主机是否支持pptp，返回结果为yes就表示通过。
modprobe ppp-compress-18 && echo yes
### 先更新一下再安装。
yum install update
### 安装ppp和pptpd
yum -y install ppp pptpd
### 配置pptpd
1.编辑配置文件 vi /etc/pptpd.conf
localip为vpn的网关地址
remoteip为vpn拨号获取的地址段，即在cmd里ipconfig里，ppp适配器vpn 192.168.8.2
```
localip 192.168.8.1
remoteip 192.168.8.2-254

```
2.编辑 vi /etc/ppp/options.pptpd,添加dns
```
ms-dns 8.8.8.8
ms-dns 8.8.4.4
```
3.配置vpn账号和密码，编辑/etc/ppp/chap-secrets
```
username1 pptpd password1 *
```
4.打开内核转发，编辑/tc/sysctl.conf
```
net.ipv4.ip_forward = 1
```
重启
sysctl -p
5.添加iptables转发规则,因为是专有网络的服务器，在服务器内部是看不到外网网卡eth1
修改/etc/sysconfig/iptables 里nat表
```
*filter
-A FORWARD -i ppp+ -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -j TCPMSS --set-mss 1356
*nat
-A POSTROUTING -s 192.168.8.0/24 -j MASQUERADE
```
重启防火墙
service iptables save
service iptables restart

### 服务配置
1.重启pptpd服务
```
service iptables restart
```
2.设置pptp和iptables自启动
```
chkconfig pptpd on
chkconfig iptables on
```
### vpn连接成功
### 遇到的问题
1.配置完成后，一直显示，无网络访问权限
[ 问题现象 ]
配置vpn后但是无法访问外网

[ 问题实例 ]
47.91.156.125

[ 处理意见 ]
目前我们检查您的主机47.91.156.125ping是正常的，您连接vpn后无法访问外网是您的配置问题导致的，具体需要您进行排查下，但是具体的您可以参考文档https://help.aliyun.com/knowledge_detail/41345.html进行核实下
按照参考文档重新设置了iptables的转发规则，成功了
2.用了两个安装pptpd vpn的脚本

### 参考链接
1.https://help.aliyun.com/knowledge_detail/41345.html
3.https://blog.linuxeye.com/412.html

### 安装FTP服务

 1. 检测是否安装了FTP # rpm -q vsftpd
 2. 安装FTP， yum install vsftpd
 3. 设置为开机启动，chkconfig vsftpd on
 4. 配置ftp参数
    ```
    # setsebool -P ftpd_disable_trans=1

    修改/etc/vsftpd/vsftpd.conf,在最后一行处添加local_root=/
    service vsftpd restart
    
    #mkdir /tmp/test  创建一个ftp上传时的目录
    //1、创建一个账号为test的账户，-s /sbin/nologin是让其不能登陆系统，-d 是指定用户目录为/tpm/test ，即该账户只能登陆ftp，却不能用做登陆系统用。
    #adduser -d /tmp/test  -g ftp -s /sbin/nologin test 

    #passwd test
    
    修改/etc/vsftpd/vsftpd.conf
    chroot_list_enable=YES
    chroot_list_file=/etc/vsftpd/chroot_list

    新增一个文件: /etc/vsftpd/chroot_list
    内容写需要限制的用户名：
    test

    重新启动vsftpd
    #service vsftpd restart
    ```
 4. 文件上传 
 ```
 lcd d:/frontend/baby/common2015/ 
 cd /tmp/test/
 chmod -R 777 /tmp/test
 put project.json
 //可以上传一个tar.gz的压缩包，上传到服务器后，再解压缩
 ```
 5. 文件下载
  ```
 lcd d:/frontend/baby/common2015/ 
 cd /tmp/test/
 get index.html
 ```
 6.客户端ftp命令行
 win+R ftp 打卡ftp命令行
 open 47.91.156.125 打开ftp服务器，输入用户名， 密码
 
 lcd 进入客户端的文件夹
 !dir 显示客户端当前目录的文件信息
 cd 进入服务器的文件夹
 dir 显示服务器端当前目录的文件信息
 get 下载一个文件
 mget *.* 下载当前服务器文件下的所有的文件
 put 上传一个文件
 mput *.* 上传全部文件，不包括文件夹
 bye 退出ftp服务器
 ? 查看更多命令
 
 7.遇到的问题
  - 文件上传的时候，遇到vsftp上传文件出现553 Could not create file，是因为/tmp/test文件夹没有读写的权限，chmod -R 777，修改上传文件夹的读写权限 
  - 文件下载的时候， ftp报错 200 port command successful. consider using pasv 425 failed to establish connection，关闭客户端电脑的防火墙就好了

### nginx 安装步骤

 1. 检查安装环境
    ```
    //nginx 需要gcc编译
    rpm -qa gcc 
    // nginx的Rewrite和HTTP模块会哟用到PCRE
    rpm -qa pcre
    //nginx的Gzip用到zlib
    rpm -qa zlib
    rpm -qa openssl
    //还需要安装pcre-devel openssl-devel,不然后续编译的时候，会报错
    ./configure: error: the HTTP rewrite module requires the PCRE library.
 
    yum -y install pcre-devel openssl openssl-devel
    ```
 2. 下载nginx
 ```
 //下载
 cd /usr/local/src
 wget http://nginx.org/download/nginx-1.10.3.tar.gz
 //解压
 tar -zxvf nginx-1.10.3.tar.gz
 //安装环境检查和安装配置
 cd nginx-1.10.3
 ./configure --prefix=/usr/local/nginx
 //编译
 make
 //安装
 make install
 //查看安装
 whereis nginx
 //启动nginx
  /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
 //查看nginx进程状态
 ps -A | grep nginx
 //打开ssi
 server{
    location / {
    ssi on;
    ssi_silent_errors on;
    }
 }
 //重启nginx
 cd /usr/local/nginx/sbin
 ./nginx -s reload
 ```
 2. 参考链接
    http://www.cnblogs.com/heacool/p/6406664.html
    http://williamx.blog.51cto.com/3629295/958398

### 安装nodejs环境

 1. 安装nvm
 ```
 //https://github.com/creationix/nvm
 wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash
 //打印NVM_DIR
 echo $NVM_DIR
 ```
 2. 安装nodejs
 3. nvm命令
    - nvm list-remote：列出所有可安装的版本；
    - nvm install v0.12.4：安装指定版本的nodejs
    - nvm ls 查看当前已安装的版本
    - nvm use v0.12.4 切换版本
    - nvm alias default v0.12.4 设置默认版本    
 4. nodejs项目部署
 在服务器上创建测试代码
```javascript
const http = require('http')
const hostname = '127.0.0.1';//专有网络不能填写公网ip地址
const port = 9000;
const server = http.createServer((req, res) => {
    res.statusCode = 200;
    res.end('hello, this is my first test nodejs project');
});
server.listen(port, hostname, () => {
    console.log(`Server running at http://${hostname}:${port}/`);
});
```
还需要配置nginx,并重启
```
location / {

   proxy_pass http://127.0.0.1:9000;

}
```


### pptpd vpn 查看当前在线用户
```
last | grep still | grep ppp
```
### 命令
- 系统
    uname -a 查看内核/操作系统/CPU信息
    hostname 查看计算机名
    clear 清屏
    who 显示在线登录用户
    whoami 显示当前操作用户
    env 查看环境变量
- 资源
    free -m 查看内存使用量和交换区使用量
    df -h 查看各分区使用情况
    du -sh <目录名> 查看指定目录大小
    grep MemTotal /proc/meminfo 查看内存总量
    grep MemFress /proc/meminfo 查看空闲内存量
    uptime 查看系统运行时间、用户数、负载
    cat /proc/loadavg 查看系统负载
- 用户
    w 查看活动用户
    last 查看用户登录日志

- 进程
    ps -ef 查看所有进程
    top 动态显示当前耗费资源最多的进程信息
- 服务
    chkconfig --list 列出所有系统服务
    chkconfig --list | grep on 列出所有启动的系统服务
- 程序
rpm  -qa 查看所有安装的软件包

### 安装Git
```bash
yum install git
git --version
yum remove git //卸载git
```
## 安装L2TP VPN
wget https://git.io/vpnsetup-centos -O vpnsetup.sh && sudo sh vpnsetup.sh
IPsec PSK: eRc...

查看秘钥
vi /etc/ipsec.secrets

// 添加VPN账号
/etc/ppp/chap-secrets

https://github.com/hwdsl2/setup-ipsec-vpn/blob/master/docs/clients-zh.md#os-x
