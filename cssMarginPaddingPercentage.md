---
title: CSS中百分比值
date: 2017-05-22 10:25:46
tags: CSS
---
* line-height
* 定位中的百分比
如果top|bottom|left|right的值为百分比
top|bottom根据包含块的高度计算百分比的值
left|right根据包含块的宽度计算百分比的值
* margin|padding
行内元素margin|padding值,左右能设置，上下不能设置
如果元素的padding或margin值是百分比值，那么，它的值是根据父元素的宽度来计算的
可以利用此属性，用css实现自适应的正方形

```html
//方案一:利用padding-top
.square{
  width:100%;
  padding-top:100%;
  background:#000;
}
//方案二:利用伪类
.square3{
  width:20%;
  background:yellow;
}
.square3:after{
  content:'';
  display:block;
  padding-top:100%;
}
.square4{
  width:20%;
  background:green;
  overflow:hidden;
}
.square4:after{
  content:'';
  display:block;
  margin-top:100%;
}
//方案三:利用vw单位
.square{
  width:100vw;
  height:100vw;
}
```

```html
<p><a><img width='2000px' height='2000px' /></a></p>
//1.页面中这三个元素的宽高是多少，img能把a标签撑起来么，为什么
//2.img设置position:absolute; top:10%;left:10%; img的位置，top/left的百分比是基于谁的
//3.如果p设置position:relative;img的position:absolute; top:10%;left:10%; img的位置，top/left的百分比是基于谁的
```
