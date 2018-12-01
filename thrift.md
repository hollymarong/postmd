---
title: thrift
date: 2017-11-21 19:08:24
tags: thrift
---


## 
内网服务间API调用http协议，请求路径长且重度依赖DNS+MGW+Nginx,存在单点风险。业务核心链路上大面积依赖DNS+MGW+Nginx，对服务的可用性带来巨大隐患。
http请求效率比不上thrift
### 用脚本编译thrift文件
```
./genthrift.sh
```
* 编译过程报错，需要后端修改IDL文件

```
thrift特殊处理int64 long行数字
http接口处理特殊long型数字
```
