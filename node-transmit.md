---
title: NodeJs转发请求
date: 2017-04-18 16:59:27
tags: JavaScript

抓取非utf-8编码的页面，需要使用iconv-lite转换编码,代码如下
news.163.com页面采用的是GBK的编码

需要安装的依赖包：
---
```bash
npm install --save url iconv-lite
```
代码如下：
```javascript
var http = require('http');
var url = require('url');
var iconv = require('iconv-lite');

http.createServer(function(request, response){
    var pathname = url.parse(request.url).pathname;
    var content = '';
    var opt = {
        host: '163.com',
        port: '80',
        method: 'GET',
        path:pathname
    };
    var req = http.request(opt, function(res){
        res.setEncoding('binary');
        res.on('data', function(data){
            console.log('return');
            content += data;
        }).on('end', function(){
            var buf = new Buffer(content, 'binary');
            var html = iconv.decode(buf, 'GBK'); //将GBK编码的字符转为utf8
            console.log(html)
            response.writeHead(200, {'Content-Type': 'text/html;charset=utf-8'});
            response.write(html);
            response.end();
        }).on('error', function(e){
            console.log('error: ' + e.message)
        });
    });
    req.end();
}).listen(8000);
```
