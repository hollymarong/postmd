---
title: hexo + Github搭建自己的博客
date: 2018-03-01 21:11:47
tags: blog
---

* 新建文章
```
hexo n (hexo new) [filename]
```
* 部署到Github
``` 
hexo g (hexo generate) # 生成
hexo s (hexo server) # 启动本地web服务器
hexo d (hexo deploy)# 部署到Github
```

* 插入本地图片
图片可以放在文章目录中，文章的目录需要配置_config.yml文件,将post_asset_folder设置为true
```
post_asset_folder: true
```
执行命令 hexo new [filename],会在_posts文件夹中生成filename.md, filename文件夹, 将图片资源放到filename文件夹中，就可以在文章中引用。具体引用方式如下：
```
// 只在文章中显示
![](image.jpg)
// 在文章和首页中同时显示
{% asset_img image.jpe This is an image %}
```


参考文章：
https://yanyinhong.github.io/2017/05/02/How-to-insert-image-in-hexo-post/
