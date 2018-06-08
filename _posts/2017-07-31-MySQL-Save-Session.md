---
layout: post
title:  "数据库中保存会话"
date:   2017-07-31 19:43:35 +0200
category: [php]
tags: [php,MySQL]
---

> 在PHP中我们常常使用 SESSION 保存会话机制，SESSION通常会将会话数据保存到服务器文本文件中（通常在系统的临时目录，如UNIX里的/tmp目录），文件名匹配会话ID。除了这一种机制管理会话之外，PHP还提供另一种机制——在数据库中存储会话数据。

使用数据库存储会话机制原因有下：

- 提高系统安全性：很多个程序同时需要访问临时目录中的会话信息，难免会引发数据安全问题；
- 可以更方便的搜索Web站点更多的会话信息；
- 多服务器时可以更方便的访问会话信息：同一个用户可以在一次会话中对不同服务器的多个页面发起请求；

## 具体操作
### 1、建立会话表
这个表可以在某个已有的数据库中创建，也可以在单独的数据库中创建，能够连接查询到就行。
会话表至少需要三个字段，正常生产环境下还要必须一个用户ID字段（因情况而定），如下：

字段类型 | 保存的数据
---|---
CHAR(32) | 会话ID
TEXT | 会话数据
TIMESTAMP | 会话数据最后一次访问时间

```
CREATE TABLE sessions (
    id CHAR(32) NOT NULL,
    data TEXT,
    last_accessed TIMESTAMP NOT NULL,
    PRIMARY KEY (id)
);
```
### 2、定义会话函数
这里将用到session_set_save_handler()函数来让PHP使用我们自定义的与数据库交互的函数。
session_set_save_handler()函数需要6个参数，如下：

次序 | 函数被调用的时机
---|---
1 | 启动会话
2 | 关闭会话
3 | 读取会话数据
4 | 写入会话数据
5 | 销毁会话数据
6 | 消除旧数据
所以我们需要6个相应的函数，示例代码如下：


```
$sdbc = NULL;  // 将用来存储数据库连接
/**
* 打开会话
* @return true;
*
*/
function open_session(){
    global $sdbc;
    // 主机、数据库账号、数据库密码、sessions表所在数据库名称
    $sdbc = mysqli_connect('localhost','root','password','test');
    return true;
}

/**
* 关闭会话
*/
function close_session(){
    global $sdbc;
    return mysqli_close($sdbc);
}


/**
* 读取session函数
* @param $sid 会话ID
* @return $data | ''
*/
function read_session($sid){
    global $sdbc;
    $q = sprintf('SELECT data FROM sessions WHERE id = "%s"' , mysqli_real_escape_string($sdbc , $sid));
    $r = mysqli_query($sdbc , $q);
    if(mysqli_num_rows($r) == 1){
        list($data) = mysqli_fetch_array($r ,MYSQLI_NUM);
        return $data;
    }else{
        return '';
    }
}

/**
* 写入session函数
* @param $sid 会话ID
* @param $data 会话数据
* @return true;
*/
function write_session( $sid , $data ){
    global $sdbc;
    $q = sprintf('REPLACE INTO sessions (id , data) VALUES ("%s" ,"%s")' , mysqli_real_escape_string($sid , $data));
    $r = mysqli_query($sdbc , $q);
    
    return true;
}

/**
* 销毁session函数
* @param $sid 会话ID
* @return true
*/
function destroy_session($sid){
    global $sdbc;
    $q = sprintf('DELETE FROM sessions WHERE id = "%s"',mysqli_real_escape_string($sdbc , $sid));
    
    $r = mysqli_query($sdbc , $q);
    
    // clear session
    $_SESSION = array();
    
    return ture;
}

/**
* 消除旧数据
* @param $expire
* @return ture
*/
function clean_session($expire){
    global $sdbc;
    
    $q = sprintf('DELETC FROM sessions WHERE DATE_ADD(INTERVAL "%d" SECOND) < NEW()',(int)$expire);
    
    $r = mysqli_query($sdbc , $q);
    return true;
}

/*************************/
/***** END FUNCTIONS *****/
/*************************/

session_set_save_handler('open_session','close_session','read_session','write_session','destroy_session','clean_session');

session_start();

```
[mysql_real_escape_string()](http://www.w3school.com.cn/php/func_mysql_real_escape_string.asp) 函数转义 SQL 语句中使用的字符串中的特殊字符。可使用本函数来预防数据库攻击。




