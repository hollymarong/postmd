---
title: curl
date: 2018-06-01 19:41:25
tags:
---
curl命令行模拟浏览器请求
浏览器中，右键单击，选择Copy => copy as cUrl ->在命令行中粘贴 -> curl 后需要加上i(Ctrl-a回到头部，加上i)
```
 curl -i 'http://.../mt-sso' -H ...
HTTP/1.1 200 OK
Server: Tengine
Date: Fri, 01 Jun 2018 11:44:18 GMT
Content-Length: 0
Connection: keep-alive
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: POST,GET,PUT,PATCH,DELETE
Access-Control-Max-Age: 3600
Access-Control-Allow-Headers: x-requested-with
Access-Control-Allow-Credentials: true
```
修改Origin
```
curl -i 'http://..../mt-sso' -H 'Origin:www.baidu.com' -H 
HTTP/1.1 200 OK
Server: Tengine
Date: Fri, 01 Jun 2018 11:45:26 GMT
Content-Length: 0
Connection: keep-alive
Access-Control-Allow-Origin: www.baidu.com
Access-Control-Allow-Methods: POST,GET,PUT,PATCH,DELETE
Access-Control-Max-Age: 3600
Access-Control-Allow-Headers: x-requested-with
Access-Control-Allow-Credentials: true
```
