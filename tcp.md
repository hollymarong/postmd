---
title: TCP
date: 2017-11-10 19:17:09
tags: tcp
---

TCP(Transmission Control Protocol 传输控制协议)面向连接的，可靠的，基于字节流的传输层通信协议。

{% asset_img tcp_info.jpeg This is an image %}

### 三次握手
第一次握手:客户端发送连接请求报文段，SYN位置为1，Sequence Number为x;然后，客户端进入SYN_SEND状态，等待服务器的确认。
第二次握手:服务器收到SYN报文段，需要对这个SYN报文段进行确认，设置Acknowledgment Number为x+1(Sequence Number+1)；同时自己发送SYN请求信息，SYN位置为1，Sequence Number为y；服务器将上述所有信息放到一个报文段（SYN+ACK报文段）中，一并发给客户端，此时服务器进入SYN_RECV状态。
第三次握手:客户端收到服务端的SYN+ACK报文段，然后将Acknowledgment Number设置为y+1,向服务器发送ACK报文段，这个报文段发送完毕后，客户端和服务端都进入ESTABLISHED状态，完成TCP三次握手。
完成了三次握手，客户端和服务端就可以开始传送数据。

### 四次分手
第一次分手:主机1（可以是客户端，也可以是服务器端）,设置Sequence Number和Ackowledgment Number，向主机2发送一个FIN报文段；此时，主机1进入FIN_WAIT_1状态；这表示主机1没有数据要发送给主机2了；
第二次分手:主机2收到了主机1发送的FIN报文段，向主机1回一个ACK报文段，Acknowledgment Number为Sequence Number加1；主机1进入FIN_WAIT_2状态；主机2告诉主机1，同意你的关闭请求；
第三次分手:主机2向主机1发送FIN报文段，请求关闭连接，同时主机2进入LAST_ACK状态；
第四次分手:主机1收到主机2发送的FIN报文段，向主机2发送ACK报文段，然后主机1进入TIME_WAIT状态；主机2收到主机1的ACK报文段以后，就关闭连接；此时，主机1等待2MSL后依然没有收到回复，则证明Server端已正常关闭，那么，主机1就可以关闭连接了。

### 参考链接
https://github.com/jawil/blog/issues/14
