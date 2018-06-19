---
layout: post
title:  "git出现'Updates were rejected because the tip of your current branch is behind...'的解决办法"
date:   2018-06-19 11:03 +0200
categories: [web]
tags: [git]
---

有如下几种解决方法：

1.使用强制push的方法：

     
    $ git push -u origin master -f 
    
这样会使远程修改丢失，一般是不可取的，尤其是多人协作开发的时候。

2.push前先将远程repository修改pull下来

    $ git pull origin master
    $ git push -u origin master 


3.若不想merge远程和本地修改，可以先创建新的分支：

    $ git branch [name]

然后push

    $ git push -u origin [name]