---
layout: post
title:  "SASS安装与入门"
date:   2017-07-31 19:43:35 +0200
category: [sass]
tags: [sass,css]
---

> Sass号称世界上最成熟、稳定和强大的CSS扩展语言，它扩展了CSS3，增加了规则、变量、混入、选择器、继承等等特性，它生成良好格式化的CSS3代码易于组织和维护。

## Sass？

> Sass(Syntactically Awesome StyleSheets)是一种CSS开发工具，提供了许多便利写法，大大节省了设计者的时间，使得CSS开发变得简单和可维护。

## 安装

安装Sass之前需要安装Ruby，因为Sass是由Ruby语言写的（不会Ruby不影响使用）。安装Ruby就不说了（网上一搜教程一大堆），安装好Ruby之后就要打开cmd（windows操作系统），敲一下命令了：
```
gem install sass
gem install compass
```

如果安装太慢，可以切换安装源，将自带的gem源改为国内的淘宝源
```
gem source --remove https://rubygems.org/
gem source -a https://ruby.taobao.org/
gem source -l
```

## 入门
### 编译

```
// 将input.scss编译成output.css
sass input.scss output.css

// 监视单个 Sass 文件，每次修改并保存时自动编译
sass --watch input.scss:output.css

// 监视整个文件夹
sass --watch app/sass:public/stylesheets
```
### 编码格式
Sass 首先检查 Unicode byte order mark，然后是 @charset 声明，最后是 Ruby string encoding，假如都没有检测到，默认使用 UTF-8 编码。
与 CSS 相同，使用 @charset 可以声明特定的编码格式。在样式文件的起始位置（前面没有任何空白与注释）插入 @charset "encoding-name"， Sass 将会按照给出的编码格式编译文件。注意所使用的编码格式必须可转换为 Unicode 字符集。
Sass 以 UTF-8 编码输出 CSS 文件，当且仅当编译后的文件中包含非 ASCII 字符时，才会在输出文件中添加 @charset 声明，而在压缩模式下 (compressed mode) 使用 UTF-8 byte order mark 代替 @charset 声明语句。

### 变量
sass可以使用变量，便于维护与修改
SassScript 最普遍的用法就是变量，变量以美元符号开头，赋值方法与 CSS 属性的写法一样：
```
$width: 5em;
```
直接使用就可以调用变量
```
#main {
  width: $width;
}
```
案例如下：
```
//sass style
//-----------------------------------
$fontStack:    Helvetica, sans-serif;
$primaryColor: #333;

body {
  font-family: $fontStack;
  color: $primaryColor;
}
```

编译之后
```
//css style
//-----------------------------------
body {
  font-family: Helvetica, sans-serif;
  color: #333;
}
```

### 嵌套
sass可以进行选择器嵌套。

```
//sass style
//-----------------------------------
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li { display: inline-block; }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}
```

编译之后
```
//css style
//-----------------------------------
nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

nav li {
  display: inline-block;
}

nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none;
}
```

### 导入
sass导入其他sass文件之后编译，优于css的@import

```
//sass style
//-----------------------------------
// _reset.scss

html,
body,
ul,
ol {
   margin: 0;
  padding: 0;
}
```

```
//sass style
//-----------------------------------
// base.scss 

@import 'reset';

body {
  font-size: 100% Helvetica, sans-serif;
  background-color: #efefef;
}
```

编译之后
```
//css style
//-----------------------------------
html, body, ul, ol {
  margin: 0;
  padding: 0;
}

body {
  background-color: #efefef;
  font-size: 100% Helvetica, sans-serif;
}
```
### mixin

sass中可用mixin定义一些代码片段，且可传参数，方便日后根据需求调用。从此处理css3的前缀兼容轻松便捷。

```
//sass style
//-----------------------------------
@mixin box-sizing ($sizing) {
    -webkit-box-sizing:$sizing;     
       -moz-box-sizing:$sizing;
            box-sizing:$sizing;
}
.box-border{
    border:1px solid #ccc;
    @include box-sizing(border-box);
}
```

编译之后
```
//css style
//-----------------------------------
.box-border {
  border: 1px solid #ccc;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
```

### 扩展/继承
sass可通过@extend来实现代码组合声明，使代码更加优越简洁。

```
//sass style
//-----------------------------------
.message {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.success {
  @extend .message;
  border-color: green;
}

.error {
  @extend .message;
  border-color: red;
}

.warning {
  @extend .message;
  border-color: yellow;
}
```

编译之后
```
//css style
//-----------------------------------
.message, .success, .error, .warning {
  border: 1px solid #cccccc;
  padding: 10px;
  color: #333;
}

.success {
  border-color: green;
}

.error {
  border-color: red;
}

.warning {
  border-color: yellow;
}
```

### 运算
sass可进行简单的加减乘除运算等
```
//sass style
//-----------------------------------
.container { width: 100%; }

article[role="main"] {
  float: left;
  width: 600px / 960px * 100%;
}

aside[role="complimentary"] {
  float: right;
  width: 300px / 960px * 100%;
}
```

编译之后
```
//css style
//-----------------------------------
.container {
  width: 100%;
}

article[role="main"] {
  float: left;
  width: 62.5%;
}

aside[role="complimentary"] {
  float: right;
  width: 31.25%;
}
```
### 颜色
sass中集成了大量的颜色函数，让变换颜色更加简单。
```
//sass style
//-----------------------------------
$linkColor: #08c;
a {
    text-decoration:none;
    color:$linkColor;
    &:hover{
      color:darken($linkColor,10%);
    }
}
```

编译之后
```
//css style
//-----------------------------------
a {
  text-decoration: none;
  color: #0088cc;
}
a:hover {
  color: #006699;
}
```

