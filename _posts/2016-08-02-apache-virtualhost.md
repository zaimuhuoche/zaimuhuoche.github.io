---
layout: post
title: Apache 2.4 配置虚拟主机
category: blog
description: 介绍 Linux/Apache 虚拟主机的配置
---

[参考文档](http://httpd.apache.org/docs/2.4/mod/directives.html)

环境：

```bash
⇒  apachectl -v
Server version: Apache/2.4.18 (Unix)
Server built:   Feb 20 2016 20:03:19
```

0x01：

```bash
⇒  sudo vim /etc/apache2/httpd.conf

# 取消注释
# Include /etc/apache2/extra/httpd-vhosts.conf

⇒  sudo vim /etc/apache2/extra/httpd-vhosts.conf

# 参考代码块配置
> <VirtualHost *:80>
>     ServerName www.test.com
>     ServerAlias www.test.com
>
>     ServerAdmin xus898@outlook.com
>     DocumentRoot "/Users/xus/Documents/local_test"
>     <Directory "/Users/xus/Documents/local_test">
>         Options Indexes FollowSymLinks MultiViews
>         AllowOverride None
>         Order allow,deny
>         Allow from all # Deny from all
>     </Directory>
>
>     ErrorLog "/private/var/log/apache2/test_error_log"
>     Customlog "/private/var/log/apache2/test_access_log" common
> </VirtualHost>

⇒  sudo vim /etc/hosts # 配置域名

# 参考配置
# 127.0.0.1   www.test.com
# 127.0.0.1   local.test.com

⇒  sudo apachectl restart
```

0x02：You don't have permission to access / on this server.

```bash
# httpd.conf 片段
<Directory />
     AllowOverride none
     Require all denied # Require all granted
</Directory>

修改为：
<Directory />
    Options Indexes FollowSymLinks
    AllowOverride None
</Directory>

⇒  sudo apachectl restart
```

0x03：CentOS

环境：

```bash
系统版本：CentOS 6.7
安装方式：yum
默认错误日志路径：/var/log/httpd/error_log

⇒  apachectl -v
Server version: Apache/2.2.15 (Unix)
Server built:   May 11 2016 19:28:33

# /etc/httpd/conf/httpd.conf
ServerName localhost:80
```

启动错误1：Permission denied: httpd: could not open error log file /home/www/log/error_log.

```bash
# 临时
setenforce 0    # 设置SELinux 成为permissive模式
setenforce 1    # 设置SELinux 成为enforcing模式

# 永久
/etc/selinux/config
SELINUX=enforcing
SELINUX=disabled
```

0x04：ubuntu

---

2016-08-04 15:14:09 Post