---
layout: post
title:  "关于redis在laravel中设置过期时间的使用纠正"
date:   2018-06-08 12:57 +0200
tags: [php,Redis]
category: [web]
---

> 之前在Laravel中初次使用Redis的时候，不知道怎么设置过期时间，因为有很多地方会希望Redis存储一段时间之后自动被清除，所以在网上搜了一下方法，很多地方说的不太正确，以至于走了一次坑，在此记录一下。


（String）过期时间正确设置方法：

```Redis::setex( $key , $expired_at , $value );```

$expired_at 就是过期时间，单位秒