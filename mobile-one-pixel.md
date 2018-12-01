---
title: 移动端1px的问题
date: 2017-06-03 16:12:28
tags: Mobile
---

移动端下，一般设置1px的边线，但在高清屏幕的手机中浏览时，会变粗，并没有显示1px。
* 设备像素(物理像素)
设备像素又称物理像素，是显示设备中一个最微小的物理部件。每个像素可以根据操作系统设置自己的颜色和亮度。是固定不变的值
* 设备独立像素(device-independent pixel)
设备独立像素也称为密度无关像素，可以认为是计算机坐标系统中的一个点，这个点代表一个可以由程序使用的虚拟像素(比如CSS像素)，然后由相关系统转换为物理像素。
* CSS像素
CSS像素是抽象的，可以收缩放放大,主要使用在浏览器山个。其面积随着浏览器的缩放比例而同步缩放
* 屏幕密度
是指一个设备表面上存在的像素数量，通常以每英寸有多少像素来计算(PPI).
* devicePixelRatio 设备像素比
devicePixelRatio = 屏幕物理像素/设备独立像素(dips: device-indipendent pixels)
devicePixedlRatio为1时是普通显示屏，大于1时为高清显示屏
在Javascript中，可以通过window.devicePixelRatio获取当前设备的dpr
在CSS中，可以通过-webkit-device-pixel-ratio,-webkit-min-device-pir
* viewport
- visual viewport(视觉视口)
visual viewport = screen.width/initail-scale
320px宽的屏幕可以显示980px宽的内容
通过设置initial-scale的值可以设置visual viewport的宽度
visual viewport的宽度变化会影响layout viewport的变化
用户能看到的网站区域
window.innerWidth/window.innerHeight是获取视觉视口的尺寸
- layout viewport(布局视口)
document.documentElement.clientWidth/document.clientHeight是获取布局视口的尺寸
默认情况下，layout viewport一般为980px
<meta name="viewport" content="width=device-width">
设置布局视口的宽度为屏幕宽度
布局视口的宽度大于等于视觉视口
- ideal viewport(理想视口)
布局视口的默认宽度并不一定是一个理想的宽度，它对设备来说是最理想的布局视口尺寸。只有主动地往页面中添加meta视口标签,理想视口才会生效。
window.screen.width/window.screen.height是理想视口的尺寸
当设备方向改变时，iphone的理想视口的尺寸值不会改变，andrio设备的理想视口的尺寸会发生改变
## 1px变粗的原因
* CSS中的1px并不等于设备中的1px，不同的屏幕有不同的设备像素比
例如iphone4s 屏幕物理像素：640 设备独立像素：320,devicePixelRatio: 2, 1像素在iphone4s中会占4个物理像素，由四个像素点所绘制。
## 解决方案
### 利用transform缩放
```javascript
   //javascript 对于devicePixelRatio值为3的情况，不会特意设置，统一采用值为2的情况
   if(window.devicePixelRatio && window.devicePixelRatio >= 2) {
       document.querySelector('html').className = 'hd-screen';
   }
   //css
     .one-pixel{
         position:relative;
         height:200px;
     }
     .one-pixel:after{
         content:'';
         position:absolute;
         bottom:0;
         left:0;
         width:100%;
         height: 1px;
         background:#000;
     }
     .hd-screen .one-pixel:after{
         transform: scaleY(0.5);
         transform-origin: 0 0;
     }
```
### 利用viewport
把viewport的宽度设置为实际的设备物理宽度，css的1px就等于设备的1px
```javascript
var scale = 1 / window.devicePixelRatio;
var meta = document.createElement('meta');
meta.setAttribute("name", "viewport");
meta.setAttribute("content", "initial-scale=" + scale + ", maximum-scale=" + scale + ",minimum-scale=" + scale + ",user-scale=no");
```
### 利用border-image

## 参考链接
https://github.com/amfe/article/issues/17
https://segmentfault.com/a/1190000008619063
