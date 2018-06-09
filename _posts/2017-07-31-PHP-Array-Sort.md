---
layout: post
title:  "PHP多维数组排序"
date:   2017-07-31 13:25:35 +0200
no-post-nav: true
category: [web]
tages: [php]
---

> 多维数组在PHP中被用到，处理数据库数据以及其他数据整理等等。或许我们可以随便写一个函数进行相应的操作处理，但不一定就是最简单最高效的方法。看过了《深入理解PHP——高级技巧、面向对象与核心技术》第三版（原版为《PHP Advance and Object-Oriented Programming Visual QuickPro Guide》Third Edition）之后深受感触。希望记录下来以备使用。

## 多维数组排序
PHP排序我们都知道内置了很多函数，比如==sort()==、==ksort()==、==asort()==、==rsort()== 等等，让我们能够很快的处理一维数组排序问题，但是对于二维数组，这些内置的排序函数就没有什么作用了，所以需要内置函数 ==usort()==、==uksort()==、==uasort()== 用我们自己定义的函数排序。比如：
```
$arr = array(
    array('key1'=>123,'key2'=>234,'key3'=>456),
    array('key1'=>39,'key2'=>96,'key3'=>12),
    array('key1'=>99,'key2'=>18,'key3'=>32)
);

/** 
*
*  自定义排序函数
*  @功能 让数组通过'key1'进行排序
*
**/
function asc_number_sort($x , $y){
    if($x['key1'] > $y['key1']){
        // 返回true就是将第一个参数排在前面
        return true;
    }elseif($x['key1'] < $y['key1']){
        // 返回false就是将第一个参数排在后面
        return false;
    }
    else{
        // 返回0就是这两个参数相等
        return 0;
    }
}

// 调用函数
usort($arr , 'asc_number_sort');

// 打印函数
echo "<pre>";
print_r($arr);
```
返回结果如下:
```
Array
(
    [0] => Array
        (
            [key1] => 39
            [key2] => 96
            [key3] => 12
        )

    [1] => Array
        (
            [key1] => 99
            [key2] => 18
            [key3] => 32
        )

    [2] => Array
        (
            [key1] => 123
            [key2] => 234
            [key3] => 456
        )

)
```
通过此案例可以扩展到对其他键名、键值排序，也可对字符串进行排序（需要用到字符串比较函数 ==strcasecmp()== 、==strcmp()== 等等）。同样也可以应用到数据库检索出的数组处理中。


