---
layout: post
title: Record for blog construction(Continue to update...)
date: 2015-05-09
thumbnail:
tags: Blog Problem_Shooting
categories: Blog
published: true

---

本文旨在记录本博客在搭建过程中遇到的问题，及相应的解决方案，希望能对他人有帮助。


_目前存在的问题：_

* Categories\Tags两个页面需重新设计
* 个人网站的路径问题
* Disqus 在首页一直是loading状态（路径问题）
* 购买域名
* 网页的图标设计 
* RSS feed 
* 字体大小需要微调(用python批处理)
* google analysis 


_已解决：_   

* 行内代码引用部分在首页出现“泄漏”情况，把下面的文字全部染红
    * 问题：index页面在截取摘要时恰好截取到一般的行内代码，导致其没有结束
    * 解决方案：在`_config.yml`文件中设置摘要分割符，即添加一行：`excerpt_separator: "\n\n"`
* 每次浏览，字体大小、类型都不一样
    * 问题：font-family设置混乱，有多个css
    * 解决方案：删除无用的css，修改font-family
* 首页翻页按钮需重新设计，包括位置、外形
    * 解决方案：给div添加属性style="text-align:center"
* Markdown 的 strikethrough失效
    * 解决方案：修改`_config.yml`文件：
        ```
        markdown: redcarpet
        redcarpet:
          extensions: ["strikethrough"]
        ```
    * [Jekyll markdown strikethrough](http://stackoverflow.com/questions/17004095/jekyll-markdown-strikethrough)


_Last Modification By Tsien @ 2015/06/17 10:00_