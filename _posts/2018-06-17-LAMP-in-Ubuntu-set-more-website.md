---
layout: post
title:  "LAMP在ubuntu系统中配置多虚拟主机站点"
date:   2018-06-17 16:11 +0200
categories: [web]
---
### 1. 增加配置文件

1. ```cd /etc/apache2/sites-available```
2. ```cp 000-default.conf 001-default.conf ```
3. ```vim 001-default.conf  ```
4. 编写内容(按```i```键编辑)

    - ServiceName 填写站点域名
    - DocumentRoot 填写网站根目录
    - Directory 填写网站根目录
```
<VirtualHost *> 
         ServerAdmin webmaster@localhost 
         ServerName www.xxx.com  
         CustomLog   /var/log/apache2/site1.xxxx.com-access.log combined 
         DocumentRoot /var/www/site1/ 
         <Directory /var/www/site1/> 
                 Options Indexes FollowSymLinks MultiViews 
                 AllowOverride all 
                 Order allow,deny 
                 allow from all 
         </Directory> 
</VirtualHost>
```

5. 保存文件并退出编辑器(在编辑模式时，按```esc```键，然后输入```:wq```)

### 2. 创建软连接

> ```ln -s /etc/apache2/sites-available/001-default.conf /etc/apache2/sites-enabled/001-default.conf ```

### 3. 修改hosts文件

1. ```vim /etc/hosts```
2. 编写内容(按```i```键编辑)

```
127.0.0.1       localhost
127.0.0.1       www.xxx.com


# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

