---
title: Git push error解决方案
date: 2017-08-20 22:27:28
tags: [ssh,git]
categories: [Git]
---

## 操作过程
校园网`git push`推送时出现
```
ssh_dispatch_run_fatal: Connection to 192.30.255.112: Software caused connection abort
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Error: ssh_dispatch_run_fatal: Connection to 192.30.255.112: Software caused connection abort
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```
更改公钥再进行`push`依旧出错，测试SSH连接
```
$ ssh -T git@github.com
ssh: connect to host github.com port 22: Connection timed out
Hi gongzhibin! You've successfully authenticated, but GitHub does not provide shell access.
```
连接成功

git提供了https、git、ssh三种协议来读写。
运行`git config --local -e`打开配置信息。
修改其中的
```
url = git@github.com:username/repo.git
```
为https协议
```
url = https://username@github.com/username/repo.git
```

回去使用宿舍网可以SSH上传，可能是学校网络出问题了，禁用了SSH的22端口导致的。


## 参考文献
1. [提交代码到GitHub SSH错误解决方案](http://www.shenyanchao.cn/blog/2013/09/16/git-ssh-connection/)