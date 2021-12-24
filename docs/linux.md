## 账户



## apt-get7.3
``` shell
sudo apt-get install software-properties-common python-software-properties
sudo add-apt-repository ppa:ondrej/php && sudo apt-get update
sudo apt-get -y install php7.3

sudo -y apt-get install php7.3-fpm php7.3-mysql php7.3-curl php7.3-json php7.3-mbstring php7.3-xml php7.3-intl php7.3-bcmath php7.3-xmlrpc php7.3-zip

PS:安装其他扩展（按需安装）
sudo apt-get install php7.3-gd
sudo apt-get install php7.3-soap
sudo apt-get install php7.3-gmp
sudo apt-get install php7.3-odbc
sudo apt-get install php7.3-pspell
sudo apt-get install php7.3-bcmath
sudo apt-get install php7.3-enchant
sudo apt-get install php7.3-ldap
sudo apt-get install php7.3-opcache
sudo apt-get install php7.3-readline
sudo apt-get install php7.3-sqlite3
sudo apt-get install php7.3-xmlrpc
sudo apt-get install php7.3-bz2
sudo apt-get install php7.3-interbase
sudo apt-get install php7.3-pgsql
sudo apt-get install php7.3-recode
sudo apt-get install php7.3-sybase
sudo apt-get install php7.3-xsl
sudo apt-get install php7.3-cgi
sudo apt-get install php7.3-dba
sudo apt-get install php7.3-phpdbg
sudo apt-get install php7.3-snmp
sudo apt-get install php7.3-tidy
sudo apt-get install php7.3-zip
```


https://blog.csdn.net/u012322399/article/details/102777257
## 挂载云盘
``` js
$ fdisk -l #以 root 用户执行以下命令，查看磁盘名称
$ mkfs -t ext4 /dev/vdb #设置文件系统为 EXT4 为例：
$ mkdir /outssd #以新建挂载点 /data 为例
$ mount /dev/vdb /outssd #新建分区挂载至新建的挂载点
$ df -TH #查看挂载结果
$ echo '/dev/vdb       /outssd              auto    defaults        0 0' >> /etc/fstab
```
## 安装Nginx
``` js
yum install nginx

# 管理 Nginx 服务
$ systemctl start nginx  # 启动 Nginx
$ systemctl stop nginx  # 停止 Nginx
$ systemctl restart nginx  # 重启 Nginx

# 使用 `systemctl` 命令开关服务的开机自启：
$ systemctl enable nginx # 启用 Nginx 开机启动
$ systemctl disable nginx # 禁用 Nginx 开机启动
```

## Nginx配置
- mkdir -p /lsoft/nginx/vhost
- mkdir -p /lsoft/nginx/log
- cat /etc/nginx/nginx.conf
``` js
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user nginx;
worker_processes auto;
# error_log /var/log/nginx/error.log;
error_log /outssd/logs/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    # access_log  /var/log/nginx/access.log  main;
    access_log /outssd/logs/nginx/access.log;
    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;
    include /lsoft/nginx/vhost/*.conf;
    server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  _;
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }

# Settings for a TLS enabled server.
#
#    server {
#        listen       443 ssl http2 default_server;
#        listen       [::]:443 ssl http2 default_server;
#        server_name  _;
#        root         /usr/share/nginx/html;
#
#        ssl_certificate "/etc/pki/nginx/server.crt";
#        ssl_certificate_key "/etc/pki/nginx/private/server.key";
#        ssl_session_cache shared:SSL:1m;
#        ssl_session_timeout  10m;
#        ssl_ciphers HIGH:!aNULL:!MD5;
#        ssl_prefer_server_ciphers on;
#
#        # Load configuration files for the default server block.
#        include /etc/nginx/default.d/*.conf;
#
#        location / {
#        }
#
#        error_page 404 /404.html;
#            location = /40x.html {
#        }
#
#        error_page 500 502 503 504 /50x.html;
#            location = /50x.html {
#        }
#    }

}


```


## 安装 PHP-FPM
``` js
# 配置 yum 源【来源：https://webtatic.com/】
$ yum install epel-release
$ rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm

# yum 搜索源
$ yum search php72

# 安装 php
$ yum install -y php72w php72w-cli php72w-fpm

# 安装 php 扩展【https://webtatic.com/packages/php72/】
$ yum install -y php72w-mbstring php72w-xml php72w-bcmath
$ yum install -y php72w-gd php72w-mysql php72w-opcache php72w-process php72w-devel

# 查看 php 扩展
$ php -m

# 管理 PHP-FPM 服务
$ systemctl restart php-fpm  # 重启 PHP-FPM
$ systemctl start php-fpm  # 启动 PHP-FPM
$ systemctl stop php-fpm  # 停止 PHP-FPM

# 开关机自启
$ systemctl enable php-fpm # 启用 PHP-FPM 开机启动
$ systemctl disable php-fpm # 禁用 PHP-FPM 开机启动

# 确认 PHP-FPM 正常运行
$ ps aux |  grep php


#卸载当前版本php及其相关软件包
$ sudo yum remove -y php*
```

## 安装php-7.3.4
[7.4版本](https://www.xstnet.com/article-152.html)

[https://www.php.net/downloads.php](https://www.php.net/downloads.php)
1.安装依赖

``` js
$ yum install -y gcc gcc-c++  make zlib zlib-devel pcre pcre-devel  libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libxml2 libxml2-devel glibc glibc-devel glib2 glib2-devel bzip2 bzip2-devel ncurses ncurses-devel curl curl-devel e2fsprogs e2fsprogs-devel krb5 krb5-devel openssl openssl-devel openldap openldap-devel nss_ldap openldap-clients openldap-servers

7.4版本
yum -y install libxml2-devel openssl-devel sqlite-devel libcurl-devel libicu-devel gcc-c++ oniguruma oniguruma-devel libxslt-devel libpng-devel libjpeg-devel freetype-devel

```
2.解压
``` js
wget https://www.php.net/distributions/php-7.4.20.tar.bz2
[root@VM_28_96_centos ~]# tar jxvf php-7.4.6.tar.bz2
[root@VM_28_96_centos php-7.4.6]# cd php-7.4.6/
```
3.配置
``` js
./configure \
--prefix=/usr/local/php \
--exec-prefix=/usr/local/php \
--bindir=/usr/local/php/bin \
--sbindir=/usr/local/php/sbin \
--includedir=/usr/local/php/include \
--libdir=/usr/local/php/lib/php \
--mandir=/usr/local/php/php/man \
--with-config-file-path=/usr/local/php/etc \
--with-openssl \
--enable-mbstring \
--with-pdo-mysql \
--enable-fpm
```

···
laravel配置
``` js
./configure \
--prefix=/usr/local/php \
--with-config-file-path=/usr/local/php/etc \
--enable-mysqlnd \
--with-mysqli=mysqlnd \
--with-pdo-mysql=mysqlnd \
--enable-pdo \
--with-iconv-dir  \
--with-freetype \
--with-jpeg \
--with-zlib \
--enable-xml \
--enable-session \
--disable-rpath \
--enable-bcmath \
--enable-shmop \
--enable-sysvsem \
--enable-inline-optimization \
--with-curl \
--enable-mbregex \
--enable-mbstring \
--enable-intl \
--enable-pcntl \
--enable-bcmath \
--enable-ftp \
--enable-gd \
--with-openssl \
--with-mhash \
--enable-pcntl \
--enable-sockets \
--with-xmlrpc \
--with-zip \
--enable-soap \
--with-gettext \
--disable-fileinfo \
--enable-opcache \
--enable-maintainer-zts \
--with-xsl \
--enable-tokenizer \
--enable-fpm
```
缺失openssl
```
yum install openssl-devel

```


No package 'libzip' found
configure: error: Package requirements (libzip >= 0.11) were not met:

No package 'libzip' found
解决方法

这里不使用软件仓库中的libzip, 也就是不使用yum install libzip-devel,
因为版本不够, 安装方法看下面一条
Requested 'libzip >= 0.11' but version of libzip is 0.10.1
这是因为软件仓库的libzip版本过低, 需要手动编译安装

``` php
# 先删除原有的libzip
yum remove -y libzip
# 下载并手动编译安装, 自己下载到合适的位置
wget https://nih.at/libzip/libzip-1.2.0.tar.gz
# 解压
tar -zxvf libzip-1.2.0.tar.gz
# 进入到源码目录
cd libzip-1.2.0
# 配置
./configure
# 编译并安装
make && make install
# 更新依赖路径, centos版本小于8的,一定要执行下面这个命令,不然还是找不到 libzip
export PKG_CONFIG_PATH="/usr/local/lib/pkgconfig/"
# 上面一行的代码参考自 https://wujie.me/centos7-3-compile-and-install-php-7-4-1/
```

./configure 配置遇到的No package 'libxml-2.0' found缺失libxml2.0 库，解决方法：
``` js
yum -y install libxml2
yum -y install libxml2-devel
```
./configure 配置遇到的No package 'sqlite3' found，解决方法：
``` js
yum install sqlite-devel
 configure: error: Please reinstall the BZip2 distribution 解决方法：

yum install bzip2 bzip2-devel
configure: error: Package requirements (oniguruma) were not met:
```
No package 'oniguruma' found
``` js
yum install https://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/o/oniguruma-6.8.2-1.el7.x86_64.rpm
yum install https://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/o/oniguruma-devel-6.8.2-1.el7.x86_64.rpm
 ```

configure: error: Package requirements (libxslt >= 1.1.0) were not met:

No package 'libxslt' found
``` js

yum install libxslt-devel
 ```

configure: error: Package requirements (libpng) were not met:

No package 'libpng' found
``` js

yum install libpng-devel
```

报错缺东西
``` js
yum install autoconf automake libtool
```

编译安装
``` js
$ make && make install
```


在之前编译的源码包中，找到 php.ini-production，复制到/usr/local/php/etc下，并改名为php.ini
``` js
$ cp php.ini-production /usr/local/php/etc/php.ini
```

将php源码编译目录下的 sapi/fpm/init.d.php-fpm 文件拷贝到系统配置 /etc/init.d 目录下并重命名为 php-fpm
``` js
[root@localhost php-7.3.4]# cp sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm
[root@localhost php-7.3.4]# chmod +x /etc/init.d/php-fpm
```
添加 php-fpm 配置文件
将php安装目录下的 /usr/local/php/etc/php-fpm.conf.default 文件拷贝同目录下并重命名为 php-fpm.conf
``` js
[root@localhost php-7.3.4]# cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf
```
添加 www.conf 配置文件
将php安装目录下的 /usr/local/php/etc/php-fpm.d/www.conf.default 文件拷贝同目录下并重命名为 www.conf

``` js
[root@localhost php-7.3.4]# cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf
```

添加php安装目录到系统环境变量
创建并打开文件php.sh

``` js
[root@localhost php-7.3.4]# vim /etc/profile.d/php.sh
```
添加内容如下:
``` js
export PATH=$PATH:/usr/local/php/bin/:/usr/local/php/sbin/
```
保存并退出

:wq!
使用source立即生效刚刚添加的php环境变量
``` js
[root@localhost php-7.3.4]# source /etc/profile.d/php.sh
```
启动php-fpm
``` js
[root@localhost php-7.3.4]# service php-fpm start

安装拓展
例 cd /root/php-7.4.6/ext/openssl/
/usr/local/php/bin/phpize
./configure
make && make install


```


## 安装 Git
``` js
$ yum install -y git

$ git --version # 查看 git 版本

# 生成 SSH 秘钥
$ ls -al ~/.ssh # 查看是否 存在 `id_rsa` 与文件 `id_rsa.pub`
$ ssh-keygen -t rsa -C "server@codezha.com" # 一路回车【密码为空】
$ ls -al ~/.ssh # 再次查看是否生成成功
$ cat ~/.ssh/id_rsa.pub # 查看公钥内容
```

## 安装 Composer
``` js
$ php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" # 或者使用 `$ wget -O composer-setup.php https://getcomposer.org/installer`
$ php -r "if (hash_file('sha384', 'composer-setup.php') === 'a5c698ffe4b8e849a443b120cd5ba38043260d5c4023dbf93e1558871f1f07f58274fc6f4c93bcfd858c6bd0775cd8d1') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
$ php composer-setup.php --filename=composer --install-dir=/usr/local/bin --version=1.9.0
$ php -r "unlink('composer-setup.php');"

# 检查安装情况
$ composer --version

# 淘宝全量镜像【https://learnku.com/composer/wikis/30594】
$ composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

# composer 故障排除
https://getcomposer.org/doc/articles/troubleshooting.md#degraded-mode
```
## 安装 NodeJs
``` js
# 卸载并添加 yum 源
$ yum remove nodejs
$ yum clean all && yum makecache fast
$ yum install -y gcc-c++ make
$ curl -sL https://rpm.nodesource.com/setup_10.x | sudo -E bash -

# 安装 nodejs
$ yum install -y nodejs

# 查看安装情况
$ node -v
$ npm -v

# 添加淘宝镜像
$ npm config set registry https://registry.npm.taobao.org
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```
## 安装 yarn
``` js
$ npm install yarn -g
$ yarn -v

# 添加淘宝镜像
$ yarn config set registry https://registry.npm.taobao.org
```
## 安装 MySql
``` js
# 安装 mysql 官方 yum 源
$ rpm -ivh https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm

# 查看 MySQL yum 源
$ yum list |  grep mysql # 发现只有 mysql80 的包，这是因为没有开启 mysql57 的包

# 关闭 80 包，开启 57 包
$ yum-config-manager --disable mysql80-community
$ yum-config-manager --enable mysql57-community

# 再次查看 MySQL yum 源
$ yum list |  grep mysql # 发现有了 mysql57 的包

# 安装 mysql
$ yum install -y mysql-community-server
$ yum install -y mysql-community-client # 根据需要安装（可不装）

# 管理 mysql
$ systemctl start mysqld # 启动 mysql
$ systemctl stop mysqld # 停止 mysql

# 查看超级账户 root 临时密码
$ grep 'temporary password' /var/log/mysqld.log
l-Mv0WGiWAX2
# 修改超级账户 root 密码
$ mysql  -uroot  -p
mysql> ALTER USER "root"@"localhost" IDENTIFIED BY 'M&ys#qlRoot-2020';


mysql> create user codezha_select IDENTIFIED by 'iR3+dI0{jK';
mysql> create user codezha_edit IDENTIFIED by 'cA4+rA0@gJ';
mysql> grant all privileges on *.* to 'codezha_edit'@'%' with grant option;
mysql> use mysql;
mysql> delete from user where user='root' and host!='localhost';//禁止root远程登录
mysql> CREATE DATABASE wenjuan DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
mysql> flush privileges;
mysql> exit;
$ systemctl restart mysqld.service


```
## 安装 Redis
``` js
# [下载 fedora 的 epel 仓库](https://fedoraproject.org/wiki/EPEL/zh-cn)
$ yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

$ yum install -y redis
$ cat /etc/redis.conf # 查看 redis 配置文件【根据需要自行修改】

# 管理程序
$ systemctl start redis # 启动 redis
$ systemctl stop redis # 停止 redis

# 测试 redis
$ ps aux | grep redis # 查看 redis 启动情况
$ redis-cli # 进入 redis 交互命令
redis>  keys *
redis> exit # 退出 redis 交互命令
# 客户端程序 PhpRedisAdmin 如有需要自行安装

$ mkdir /outssd/redis/log

```

## 部署 laravel 应用
[生产环境优化](https://learnku.com/articles/25752)
``` js

$ mkdir /data/website && cd /data/website # 创建项目目录

# 使用 composer 创建 laravel 项目
# 注意：此过程中如果 composer 遇到问题： [请根据此连接进行排查...](https://getcomposer.org/doc/articles/troubleshooting.md#degraded-mode)
$ composer create-project --prefer-dist laravel/laravel blog "5.8.*"

$ cd  /data/website/blog   # 进入项目目录
$ chmod -R 777 storage/ # 设置权限
$ chmod -R 777 bootstrap/cache/ # 设置权限

# 配置 nginx 服务器
$ vim /etc/nginx/conf.d/blog.conf
# 输入以下内容 ##########################################
server {
    listen 80;
    server_name test.learnku.net;   # 此为必修改项，请替换为服务器公网 IP 或域名
    root /data/website/blog/public; # 此为必修改项，请注意指向站点根目录的 public 子目录

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";

    index index.html index.htm index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
        try_files $uri = 400;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
#######################################################

# 重启 nginx 服务器
$ systemctl restart nginx


```
