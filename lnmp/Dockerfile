FROM ubuntu:16.04
USER root
WORKDIR /root
RUN echo "deb http://cn.archive.ubuntu.com/ubuntu/ xenial main restricted universe multiverse" > /etc/apt/sources.list \
    && echo "deb http://cn.archive.ubuntu.com/ubuntu/ xenial-security main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb http://cn.archive.ubuntu.com/ubuntu/ xenial-updates main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb http://cn.archive.ubuntu.com/ubuntu/ xenial-backports main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb http://cn.archive.ubuntu.com/ubuntu/ xenial-proposed main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial-security main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial-updates main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial-backports main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial-proposed main restricted universe multiverse" >> /etc/apt/sources.list \
    && apt-get -y clean \
    && apt-get -y update \
# http://nginx.org/download/nginx-1.14.0.tar.gz
# http://cn2.php.net/distributions/php-7.0.30.tar.gz
# http://pecl.php.net/get/yaf-3.0.7.tgz
# http://pecl.php.net/get/swoole-4.0.1.tgz
# http://download.redis.io/releases/redis-4.0.10.tar.gz
# https://cdn.mysql.com//archives/mysql-5.7/mysql-5.7.10.tar.gz
#
# Ubuntu必备软件
    && apt-get -y install sudo vim curl net-tools iputils-ping git gcc make libpcre3-dev libssl-dev openssl libxslt-dev libgd-dev libgeoip-dev libcurl4-openssl-dev libmcrypt-dev libmysqlclient-dev libbz2-dev libreadline-dev libsnmp-dev libtidy-dev snmp-mibs-downloader postgresql-server-dev-all autoconf libboost-dev cmake bison libncurses5-dev\
    && chmod u+w /etc/sudoers && sed -i '/root/a liuhao  ALL=(ALL:ALL) NOPASSWD: ALL' /etc/sudoers && chmod u-w /etc/sudoers\
    && useradd -m liuhao \
RUN su liuhao && cd /home/liuhao\
 	&& curl -O http://nginx.org/download/nginx-1.14.0.tar.gz\
    && tar -zxf ./nginx-1.14.0.tar.gz && cd nginx-1.14.0/\
    && ./configure --with-cc-opt='-g -O2 -fPIE -fstack-protector-strong -Wformat -Werror=format-security -Wdate-time -D_FORTIFY_SOURCE=2' --with-ld-opt='-Wl,-Bsymbolic-functions -fPIE -pie -Wl,-z,relro -Wl,-z,now' --prefix=/home/liuhao/applications/nginx --conf-path=/home/liuhao/applications/nginx/conf/nginx.conf --http-log-path=log/access.log --error-log-path=log/error.log --lock-path=/home/liuhao/applications/nginx/lock/nginx.lock --pid-path=/applications/nginx/nginx.pid --http-client-body-temp-path=/home/liuhao/applications/nginx/body --http-fastcgi-temp-path=/home/liuhao/applications/nginx/fastcgi --http-proxy-temp-path=/home/liuhao/applications/nginx/proxy --http-scgi-temp-path=/home/liuhao/applications/nginx/scgi --http-uwsgi-temp-path=/home/liuhao/applications/nginx/uwsgi --with-debug --with-pcre-jit --with-http_ssl_module --with-http_stub_status_module --with-http_realip_module --with-http_auth_request_module --with-http_addition_module --with-http_dav_module --with-http_geoip_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_image_filter_module --with-http_v2_module --with-http_sub_module --with-http_xslt_module --with-stream --with-stream_ssl_module --with-mail --with-mail_ssl_module --with-threads\
    && make && make install\
    && cd ../\
# PHP
	# && apt-get -y install php7.0 php7.0-curl php7.0-gd php7.0-dev php7.0-opcache php7.0-zip\
    # 配置文件路径/etc/php/7.0/cli/php.ini /etc/php/7.0/fpm/php.ini /etc/php/7.0/fpm/php-fpm.conf
    && curl -O http://cn2.php.net/distributions/php-7.0.30.tar.gz\
    && tar -zxf ./php-7.0.30.tar.gz && cd php-7.0.30\
    && ./configure --prefix=/home/liuhao/applications/php --with-config-file-path=/home/liuhao/applications/php/etc --enable-fpm --with-fpm-user=liuhao --with-fpm-group=liuhao --with-mysqli --with-pdo-mysql --with-iconv-dir --with-freetype-dir --with-jpeg-dir --with-png-dir --with-zlib --with-libxml-dir=/usr --enable-xml --disable-rpath --enable-bcmath --enable-shmop --enable-inline-optimization --with-curl --enable-mbregex --enable-mbstring --with-mcrypt --enable-ftp --with-gd --enable-gd-native-ttf --with-openssl --with-mhash --enable-pcntl --enable-sockets --with-xmlrpc --enable-zip --enable-soap --without-pear --with-gettext --disable-fileinfo --enable-maintainer-zts --enable-opcache --enable-exif --enable-dba  --enable-calendar --enable-sysvmsg  --enable-sysvsem --enable-sysvshm --enable-wddx --enable-phpdbg --with-bz2=/usr --with-snmp=/usr --with-tidy=/usr --with-readline=/usr --with-pdo-pgsql=/usr\
    && mkdir -p /home/liuhao/applications/php/etc/ && cp ./php.ini-development /home/liuhao/applications/php/etc/php.ini\
    && make && make install\
    && cd ../\
    # yaf框架
	&& curl -O http://pecl.php.net/get/yaf-3.0.7.tgz\
	&& tar -zxf ./yaf-3.0.7.tgz\
	&& cd yaf-3.0.7 && /home/liuhao/applications/php/bin/phpize\
	&& ./configure --with-php-config=/home/liuhao/applications/php/bin/php-config\
	&& make && make install\
    && sed -i '911a extension=yaf.so' /home/liuhao/applications/php/etc/php.ini\
    && cd ../\
    # swoole扩展
	&& curl -O http://pecl.php.net/get/swoole-4.0.1.tgz\
	&& tar -zxf ./swoole-4.0.1.tgz\
	&& cd swoole-4.0.1 && /home/liuhao/applications/php/bin/phpize\
	&& ./configure --with-php-config=/home/liuhao/applications/php/bin/php-config\
	&& make && make install\
	&& sed -i '911a extension=swoole.so' /home/liuhao/applications/php/etc/php.ini\
    && cd ../\
# Redis
	&& curl -O http://download.redis.io/releases/redis-4.0.10.tar.gz\
	&& tar -zxf ./redis-4.0.10.tar.gz && cd redis-4.0.10\
    && make PREFIX=/home/liuhao/applications/redis install\
    && sed -i "/dir .\//c dir /home/liuhao/applications/redis/data" ./redis.conf\
    #这里设置了之后会将redis启动的提示也写入到日志文件中 && sed -i '/logfile ""/c logfile "/home/liuhao/applications/redis/log/redis.log"' ./redis.conf\
    && mkdir -p /home/liuhao/applications/redis/data\
    && mkdir -p /home/liuhao/applications/redis/etc && cp ./redis.conf /home/liuhao/applications/redis/etc/redis.conf\
    && cd ../\
# Mysql5.7
    && curl -O https://cdn.mysql.com//archives/mysql-5.7/mysql-5.7.10.tar.gz\
	&& tar -zxf ./mysql-5.7.10.tar.gz && cd ./mysql-5.7.10