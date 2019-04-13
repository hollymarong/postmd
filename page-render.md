---
title: 页面渲染
date: 2019-01-16 13:58:23
tags: browser
---
* 浏览器的主要组件包括：
 - 用户界面：用户看到的浏览器界面
 - 浏览器引擎：用来查询及操作渲染引擎的接口
 - 渲染引擎：用户显示请求的内容
 - 网络：用来完成网络的调用
 - UI后端：用来绘制选择框、对话框等基本组件
 - JS解释器：用来解释执行JS代码
 - 数据存储：客户端存储技术
 
{% asset_image browser.jpg  浏览器组件 %}

* 关键渲染路径（Critical Rendering Path）是指与当前用户操作有关的内容。
了解浏览器渲染的过程与原理，主要是为了优化关键渲染路径。

* 页面渲染流程
1. 处理 HTML 并构建 DOM 树
2. 处理 CSS 并构建 CSSOM 树
3. 将 DOM 与 CSSOM 合并成一个渲染树
4. 根据渲染树来布局
5. 将各个节点绘制到屏幕上
* 阻塞渲染：CSS 与 Javascript 
Javascript:
内联脚本：HTML解析器遇到script标签，会暂停构建DOM
脚本文件: 浏览器必须暂停，加载并执行脚本文件，如果添加async属性，会异步执行
async 与defer能改变阻塞的模式
async 与 defer 对于inline 的script都是无效的。
```
<script src="index.js" defer ></script>
<script src="index.js" async ></script>
```
defer: 延迟执行引入的js，即这段js加载时 HTML 并没有停止解析，这两个过程是并行的。整个document解析完毕且defer 的script加载完成之后，执行所有defer加载的js代码，
然后触发 DOMContentLoaded 事件。多个defer的js执行顺序是确定的。
特点：加载js文件时，不阻塞HTML的解析，执行阶段放到HTML标签解析完成之后
async: 异步执行引入的js，即这段js加载时 HTML 并没有停止解析，如果已经加载好，就会开始执行，不会等待HTML解析是否完成，依然会阻塞load事件,
async可能在DOMContentLoaded 触发之前或之后执行，但一定在load触发之前执行。多个async的js执行顺序是不确定的

CSS：
link标签会阻塞页面的渲染，浏览器会构建CSSOM。
媒体类型和媒体查询，能接触对渲染的阻塞
```
// 加载并阻塞渲染
<link href="index.css" rel="stylesheet" />

// 加载但不阻塞渲染, print只在打印网页时使用
<link href="print.css" rel="stylesheet" media="print" />

// 加载但不阻塞渲染, 会在符合条件时候阻塞渲染
<link href="small.css" rel="stylesheet" media="max-width: 800px" />
```
