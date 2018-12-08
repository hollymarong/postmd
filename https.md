---
title: HTTPS
date: 2018-12-08 11:57:46
tags: HTTPS
---
HTTPS是在HTTP和TCP之间插入了一个密码加密层（TLS活SSL）。
### HTTPS连接过程

| HTTP               | HTTPS              | 应用层     |
| ------             | ------             | ------     |
| 无                 | TSL or SSL         | 安全层     |
| TCP                | TCP                | 传输层     |
| IP                 | IP                 | 网络层     |
| 网络特有的链路接口 | 网络特有的链路接口 | 数据链路层 |
| 网络物理硬件       | 网络物理硬件       | 物理层     |

{% asset_img https_link.jpg HTTPS连接过程 %}

### Charles/Fiddler抓包https原理

{% asset_img https_charles.jpg Charles抓包原理 %}
HTTPS抓包的原理，就是Charles作为中间代理，拿到服务器证书公钥和HTTPS连接的对称密钥，与客户端和服务端进行通信。

Charles抓包的前提是客户端选择信任并安装Charles的CA证书，否则客户端会提示并中止连接，HTTPS还是很安全的。

* 参考链接：
https://zhuanlan.zhihu.com/p/22142170
https://blog.csdn.net/fox64194167/article/details/80387696
https://www.jianshu.com/p/f6b34381beac
