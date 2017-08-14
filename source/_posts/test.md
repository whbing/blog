---
title: hexo博客测试
date: 2016-09-25 22:59:50
tags: 
	- hexo
categories: 
	- 随笔生活
---

## Why
因为无聊
实在是感觉wordpress过于臃肿，而且没有好看简洁的主题，说干就干，立马就把wordpress博客内容迁移，其实wordpress也才弄了几天
那就折腾一下

## Test
试试看我写的md格式是否正确
看看next主题效果

## Note
首先，使用hexo-migrator-wordpress插件将wordpress文件迁移,对markdown的不足和常用的CSS样式进行扩展，比如图片居中，查了hexo[标签插件](https://hexo.io/zh-cn/docs/tag-plugins.html#Image)的image插件，先是没注意那个`[class name]`后来才想到这是CSS样式，并且可以自己添加自定义样式，我使用的next主题，next主题中`/source/_custom/custom.styl`可以添加自定义CSS，但是我添加的图片居中没实现，我不服啊……F12检查发现CSS样式加了下划线，往前找，发现原来的CSS加了`!important`,于是我也给自己的样式加了`!important`,呵呵……
<!--more-->
之后，为了绑定github pages的域名,需要在`/source`文件夹中新建`CNAME`文件，然后填上自己要绑定的域名；绑定coding page可以直接在page服务中绑定；写文章要想有首页文章折叠，阅读全文的按钮，可以自己在md文章中加入`<!--more-->`,API里也没看到啊，也是醉了

PS：chrome清楚缓存快捷键：`ctrl+shift+del`

接下来，就是对评论，搜索，图标的一些优化的，相信也没什么大问题了，目前看来hexo还是很简洁，用markdown格式写文章也是我想要的，所以继续加油！

**2016.9.29更新**
今天添加了swiftype搜索，并且更改了搜索框的样式；swifteype在我之前没添加sitemap时一直显示在crawling中，很是让人蛋疼，后来搜索之后才发现有[hexo-generator-sitemap](https://github.com/hexojs/hexo-generator-sitemap)这个插件，生成了网站sitemap，至此告一段落^_^


