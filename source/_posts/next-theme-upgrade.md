---
title: NexT主题升级
date: 2017-08-19 16:56:25
tags: [hexo,NexT]
category: [建站日志]
---

## 版本升级
目前升级 NexT 主题的时候并不是非常的流畅。若使用`git pull`的方式，很多时候可能会产生冲突；而下载新版本覆盖安装的方式又需要手动合并主题的`_config.yml`文件。
在此修改之前， NexT 建议将配置分离，一部分在 站点的配置文件中，另外一部分在主题的配置文件中。将需要自定的选项放置在 站点配置文件中，从而脱离避免更新主题时可能遇到的麻烦。这种方式是可行，但是有一些缺点：
1. 配置分离成了两个部分
2. 用户可能会疑惑一些选项该放置在哪里比较合适

为了解决这个问题， NexT 将会使用 Hexo 的`Data Files`。然而由于`Data Files`是在 Hexo 3 版本时引进的，所以要使用这个特性，需要 Hexo 的版本不低于 3。
<!--more-->
### 特性
通过这个特性，你可以将所有的主题配置放置在站点的`source/_data/next.yml`文件中。原先放置在站点配置文件中的选项可以迁移到新的位置，同时，主题配置文件可以不用做任何修改。若后续版本有配置相关的改动时，你仅需在`next.yml`中做相应调整即可。
### 使用
请先确保你所使用的 Hexo 版本在 3 以上
在站点的`source/_data`目录下新建`next.yml`文件（`_data`目录可能需要新建）
迁移站点配置文件和主题配置文件中的配置到`next.yml`中
## vendors文件夹404
本地预览没问题，但是deploy到github之后，主页只显示个空白背景。
原因：github page在November 3, 2016更新内容中https://github.com/blog/2277-what-s-new-in-github-pages-with-jekyll-3-3
Jekyll now ignores the vendor and node_modules directories by default.
- 解决方案一：
    手动将`source/vendors`目录修改成`source/lib`；同时，修改下主题配置文件`_config.yml`，将`_internal: vendors`修改为`_internal: lib`
- 解决方案二：
    更新作者的最新程序(不建议自己有较大改动的进行此操作)。

## 其他
1. 修改搜索菜单
2. 添加canvas
3. 添加文章更新时间

## 参考文献
1. [NexT主题升级](https://github.com/iissnan/hexo-theme-next/issues/328)
2. [vendors文件夹处理](https://github.com/iissnan/hexo-theme-next/issues/1214)