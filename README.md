# 镜像简介

 - 此镜像基础环境是CentOS 7，主要部署了httpd和PHP环境；
 - PHP的常用扩展已安装，包括：sqlite3、mysqlnd、pdo、gd、mbstring、opcache、xml、redis、mongodb等，其中redis、mongodb扩展是通过编译安装的；
 - mod_ssl也已安装，支持https；
 - 支持swoole，swoole扩展是通过编译安装的。

# 使用方法

## 简单的HTTP服务器：
``` bash
$ docker run --name httpd -d -p 80:80 -v /docker/httpd/html:/var/www/html xiaojunyi/httpd-php
```
 - 在宿主机目录`/docker/httpd/html`中添加前/后端代码文件，浏览器访问宿主机，即可看到效果了；
 - 建议同时映射以下目录：`-v /docker/httpd/html:/var/www/html -v /docker/httpd/logs:/etc/httpd/logs -v /docker/httpd/php/session:/var/lib/php/session`，这样可以方便地管理代码文件，以及查看日志，同时也可防止php的session文件在容器中生成太多；
 - 建议加上选项`--restart always`，保证httpd服务能随docker自启，和异常崩溃时能够自行重新启动。

推荐：
``` bash
$ docker run --name httpd -d --restart always -p 80:80 -v /docker/httpd/html:/var/www/html -v /docker/httpd/logs:/etc/httpd/logs -v /docker/httpd/php/session:/var/lib/php/session xiaojunyi/httpd-php
```
