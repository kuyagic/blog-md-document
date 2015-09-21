##### Web Server (Tengine)
第一步,安装依赖包  
```
apt-get install git build-essential m4 autoconf libxml2-dev libssl-dev libbz2-dev libcurl3-dev libdb5.1-dev libjpeg-dev libpng-dev libXpm-dev libfreetype6-dev libt1-dev libgmp3-dev libc-client-dev libldap2-dev libmcrypt-dev libmhash-dev freetds-dev libz-dev libmysqlclient15-dev ncurses-dev libpcre3-dev unixODBC-dev  libsqlite-dev libaspell-dev libreadline6-dev librecode-dev libsnmp-dev libtidy-dev libxslt-dev libt1-dev libpspell-dev
```
从这里下载最新的 Tengine 源码,然后在Linux环境下解压  
[戳这里下载](https://github.com/alibaba/tengine/archive/master.zip),或者直接在SSH下执行以下命令  
```
wget https://github.com/alibaba/tengine/archive/master.zip
unzip master.zip
cd tengine*
```
然后可以使用这个配置来生成编译文件`Makefile`
```
'./configure' \
'--prefix=/opt/tengine' \
'--conf-path=/opt/tengine/conf/tengine.conf' \
'--error-log-path=/var/log/tengine/error.log' \
'--http-log-path=/var/log/tengine/access.log' \
'--http-client-body-temp-path=/tmp/tengine/body/' \
'--http-proxy-temp-path=/tmp/tengine/proxy/' \
'--http-fastcgi-temp-path=/tmp/tengine/fastcgi/' \
'--http-uwsgi-temp-path=/tmp/tengine/uwsgi' \
'--http-scgi-temp-path=/tmp/tengine/scgi/' \
'--pid-path=/var/run/tengine.pid' \
'--user=www-data' \
'--group=www-data' \
'--with-ipv6' \
'--with-http_spdy_module' \
'--with-http_addition_module' \
'--with-http_sub_module' \
'--with-http_gunzip_module' \
'--with-http_gzip_static_module' \
'--with-http_secure_link_module' \
'--with-http_flv_module' \
'--with-http_slice_module' \
'--with-http_mp4_module'
```
然后,可能需要手动建立日志文件夹和临时文件夹.
```
mkdir /var/log/tengine -p
chown www-data:www-data /var/log/tengine
mkdir /tmp/tengine/{body,proxy,fastcgi,uwsgi,scgi} -p
```
最后使用
```
make
make install
```
就把 Web Server安装到了 `/opt/tengine` 目录下,配置文件夹是 `/opt/tengine/conf`  
  

##### PHP  
推荐安装 php 5.6
从这个网页下载压缩包 然后同上方法使用 `tar xf ` 来解压.  
[http://php.net/downloads.php#v5.6.13](http://php.net/downloads.php#v5.6.13)  
可以使用下面的 configure 来生成 `Makefile`
```
'./configure' \
'--prefix=/opt/php56' \
'--with-config-file-path=/opt/php56/etc' \
'--with-fpm-user=www-data' \
'--with-fpm-group=www-data' \
'--enable-fpm' \
'--enable-opcache' \
'--with-mysql=mysqlnd' \
'--with-mysqli=mysqlnd' \
'--with-pdo-mysql=mysqlnd' \
'--disable-fileinfo' \
'--with-iconv-dir=/usr/local' \
'--with-freetype-dir' \
'--with-jpeg-dir' \
'--with-png-dir' \
'--with-zlib' \
'--with-libxml-dir=/usr' \
'--enable-xml' \
'--disable-rpath' \
'--enable-bcmath' \
'--enable-shmop' \
'--enable-exif' \
'--enable-sysvsem' \
'--enable-inline-optimization' \
'--with-curl' \
'--enable-mbregex' \
'--enable-mbstring' \
'--with-mcrypt' \
'--with-gd' \
'--enable-gd-native-ttf' \
'--with-openssl' \
'--with-mhash' \
'--enable-pcntl' \
'--enable-sockets' \
'--with-xmlrpc' \
'--enable-ftp' \
'--with-gettext' \
'--enable-zip' \
'--enable-soap' \
'--disable-ipv6' \
'--disable-debug' \
'--with-xpm-dir=/usr'
```  
同样 最后用make来安装
```
make
make install
```
配置文件路径是在 `/opt/php56/etc/` 下. 可以修改prefix来指定安装位置

##### MySQL  
MySQL 简单使用源安装就可以了,简单粗暴
```
apt-get install mysql-server -y
```
期间会被要求输入数据库 `root` 用户的密码

最后,可能需要几个维护脚本来启动和停止 PHP 和 Tengine  
PHP 脚本  
```
https://copy.com/UJUdMHssipz1egvu/php.init.d
```
只需要下载到 `/etc/init.d/` 下 修改为 `php56` ,然后使用 `service php56` 就可以启动和重启了  
同理 Tengine 脚本
```
https://copy.com/gocvHDikQFKaTRUK/tengine.init.d
```
以上脚本默认安装路径都是 `/opt` , 如果修改了之前的安装路径,需要对应修改脚本一开始的路径.








