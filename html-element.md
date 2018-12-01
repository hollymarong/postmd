---
title: HTML5 Element类别和内容模型
date: 2017-06-03 12:16:40
tags: HTML
---
DOM中元素继承HTMLElement
自定义的元素继承HTMLUnknowElement
元素定义包含：
* 类别(categories)
HTML中元素被分成不同的种类，有metadata content,flow content,sectioning content,headingcontent,phrasing content,embedded content,interactive content
metadata content:
元数据内容是设置描述或其他内容的行为，或设置文档和其他文档的关系，或传递信息。
<base>,<link>,<meta>,<noscript>,<script>,<style>,<template>,<title>
flow content:
大多数元素都属于flow content
sectioning content:
分节内容是定义headings和footers范围
article,aside,nav,section
heading content:
h1,h2,h3,h4,h5,h6
phrasing content: 
定义文档的文本
phrasing content只能包含phrasing content分类的内容，不能是其他的flow content类型
<a>,<area>,<audio>,<b>,<br>,<button>,<canvas>,<code>,<data>,<datalist>,<del>,<em>,<embed>,<i>,<iframe>,<img>,<input>,<ins>,<label>,<map>,<noscript>,<object>,<output>,<picture>,<progress>,<q>,<s>,<script>,<select>,<small>,<span>,<strong>,<sub>,<sup>,<svg>,<template>,<textarea>,<video>
embedded content:
引入其他资源到文档中
<audio>,<canvas>,<embed>,<iframe>,<img>,<math>,<object>,<picture>,<svg>,<video>
interactive content:
特地用来用户交互的内容
<a>,<audio>,<button>,<embed>,<iframe>,<img>,<input>,<label>,<textarea>,<video>
palpable content
它的内容模型允许有至少一个flow content或phrasing content，它的内容都是显而易见的，没有隐藏的属性。
这并不是一个硬性的要求，在一些情况下内容可以为空，比如作为占位符出现随后脚本填充，或者是作为模板的一般分，在大多数页面中是有内容的，但在一些也页面中没有。
<a>,<address>,<article>,<audio>,<b>,<blockquote>,<button>,<canvas>,<code>,<data>,<detail>,<div>,<dl>,<em>,<embed>,<fieldset>,<figure>,<footer>,<form>,<h1>,<h2>,<h3>,<h4>,<h5>,<h6>,<header>,<i>,<iframe>,<img>,<input>,<ins>,<label>,<main>,<nav>,<ol>,<p>,<q><s>,<section>,<select>,<small>,<span>,<strong>,<sub>,<svg>,<table>,<textarea>,<ul>,<video>
script-supporting elements
不呈现内容，用来加载脚本，或为用户提供功能
<script>,<template>
* 内容模型(content-model):
所期望的内容类型，所能包含的子元素或后代元素的内容类型
有一些content-model为空的元素，可以省略end tag，如：<link>,<img>,<input>,<base>,<hr>,<meta>,<source>
transparent
<a>,<ins>,<map>,<param>,<object>
一些元素的内容模型是transparent，transparent元素的内容模型取决它的父元素。
当一个transparent的元素没有父元素，它可以接受任意flow content元素
<a><div></div></a>
上面这种嵌套是合法的，a的content-model为transparent,div的categories为flow content，所以合法
<p><a><div></div></a></p>
上面这种嵌套是不合法的，
p元素的content-model是phrasing contet，a元素类别是phrasing content, 所以p内嵌套a
a元素的content-model是transparent，对于div的是否能嵌套需要看父元素p是否能嵌套div，p元素只能嵌套Phrasing content的元素，显然不能嵌套div.
它的content-model是transparent
元素查看地址：https://html.spec.whatwg.org/#the-a-element
| element | categories                                                         | content model    |
| ---     | ---                                                                | ---              |
| p       | Flow content Palpable content                                      | Phrasing content |
| a       | Flow content Phrasing content interactive content Palpable content | transparent      |
| div     | Flow content Palpable content                                      | flow content     |
