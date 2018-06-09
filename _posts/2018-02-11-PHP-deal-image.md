---
layout: post
title:  "文本中base64位图片处理方式"
date:   2018-02-11 14:46:35 +0200
tags: [php]
categories: [web]
---
> 使用编辑器撰写文章时很多情况下都需要插入的图片，但是有一部分编辑器会将图片转为base64位编码形式保存到数据库，这种存储会造成数据库存储量庞大，所以需要在存储的时候将base64位编码图片转存到本地，并将本地图片地址替换base64位编码图片。

1. 正则表达式匹配全部base64图片
2. foreach循环：
  - 将base64图片保存到本地并获取地址
  - 将字符串中的原base64位替换成新的地址

```
/<\s*img\s+[^>]*?src\s*=\s*(\'|\")(.*?)\\1[^>]*?\/?\s*>/i
```

```
preg_match_all('/<\s*img\s+[^>]*?src\s*=\s*(\'|\")(.*?)\\1[^>]*?\/?\s*>/i',htmlspecialchars_decode($str),$match);

if(isset($match[2])){
	$base64s = $match[2];
	foreach ($base64s as $base64) {
		// Todo upload
		// ...
		
		// Todo replace
		$str = str_replace($base64, $newStr, $str);

	}
}
```