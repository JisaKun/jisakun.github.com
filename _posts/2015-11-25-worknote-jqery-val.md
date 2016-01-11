---
layout: post
title: jQuery val() vs attr() vs prop()
date: 2015-11-25 20:00
thumbnail:
tags: jQuery 前端 工作日志
categories: jQuery
published: true

---
#### Overview

今天在写页面的时候遇到了一个问题：我想实现的效果是：当一个下拉列表的值改变时，改变一个 `<input>` 标签的 value 属性。

一开始的代码是这样的: 

``` javascript
<script>
	$("select").change(function(){
		// before 
		$("input").attr("value", xxxxx);
		// after 
	})
</script>
```

结果就是 value 属性并不会百分百被修改成功。

后来我分别使用 `.val()` / `.attr("value")` / `.prop("value")` 观察赋值后的 value 属性值，发现 `.val()` 与 `.prop("value")` 会输出正确的赋值，
而 `.attr("value")` 有时会输出赋值前的值。

在网上查资料后得知，在 jQuery 的某个版本（1.6？）之后，通过 `.attr()` 获取属性值，得到的仅仅是 html 页面初始化时的值，同时用它给属性赋值，修改的仅仅是“标记”，
是初值；但是用 `.val()` 修改的是“值域”，是可以与用户交互改变的值。

StackOverflow 上是这么解释的：

> There is a big difference between an objects properties and an objects attributes
> 
> See this questions (and its answers) for some of the differences: .prop() vs .attr()
> 
> The gist is that .attr(...) is only getting the objects value at the start (when the html is created). val() is getting the object's property value which can change many times.

而 `.prop()` 同 `.val()` 。

所以讲赋值改为 `.val()` ，赋值失败的问题就消失了。

（但其实对这三个方法的机制还不是很理解）

---

#### 吐槽

写这篇文章时把一个 `<input>` 标签写成了代码，一直修改没生效，结果就是上面的页面里有一个输入框…… 太鬼畜了，不知道什么时候能好

之所以引起这个问题是因为我对前端的 DOM 和 jQuery 机制不了解，所以

我好讨厌写界面啊！诸君我讨厌前端！为什么一个（准）服务器工程师要去写界面呢……

不过想想这些简单的页面后端不写谁来写，前端也不懂 jsp 啊……

而且我司只有三个前端工程师…… 
 
---

#### Reference

[Some discuession on StackOverflow](http://stackoverflow.com/questions/8312820/jquery-val-vs-attrvalue)

P.S. StackOverflow 是个好网站。