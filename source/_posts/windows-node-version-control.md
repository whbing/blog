---
title: Windows下node版本管理
date: 2017-08-23 22:25:25
tags: [nodejs,node,nvm,nvm-windows]
categories: [Node]
---

## 原因
因为ES6的兴起，很多新特性在浏览器中实现不了，甚至低版本的node也不能实现，所以需要一个node版本管理工具，[nvm](https://github.com/creationix/nvm)不支持windows，但[nvm-windows](https://github.com/coreybutler/nvm-windows)支持，也持续在更新。

## 安装nvm-windows

1. nvm-windows发行版`nvm_noinstall.zip`[下载](https://github.com/coreybutler/nvm-windows/releases)
2. 把`nvm_noinstall.zip`解压到比如`c:/dev/nvm`中
3. 右键以管理员的身份运行`install.cmd`,直接按回车,在C盘根目录下会生成一个`setting.txt`,并拷贝到`C:/dev/nvm`，并修改内容如下:
```
root: C:\dev\nvm
path: C:\dev\nodejs
arch: 64
proxy: none
node_mirror: http://npm.taobao.org/mirrors/node/
npm_mirror: https://npm.taobao.org/mirrors/npm/
```
4. 配置环境变量
打开‘控制面板主页->高级系统设置->高级->环境变量’后会有‘用户变量’和‘系统变量’两个选项，建议在‘用户变量’里面设置：
```
NVM_HOME:C:\dev\nvm
NVM_SYMLINK:C:\dev\nodejs
```
    在PATH的最后添加`%NVM_HOME%;%NVM_SYMLINK%;`
5. npm全局安装
```
npm config set prefix "c:\dev\nvm\npm"  # 配置用npm下载包时全局安装的包路径
npm install npm -g --registry=https://registry.npm.taobao.org
npm set registry https://registry.npm.taobao.org  # 注册模块镜像 
```
6. 配置npm环境变量
变量名: `NPM_HOME`,变量值: `c:\dev\nvm\npm` 
在PATH的最后添加`%NPM_HOME%;`,要放在`NVM_SYMLINK`之前

## 参考文献
1. [windows安装nvm的两种方式](http://www.jianshu.com/p/1d80cf35abd2)
2. [npm淘宝镜像配置](https://gist.github.com/52cik/c1de8926e20971f415dd)