FROM ubuntu:16.04
RUN  echo "deb http://cn.archive.ubuntu.com/ubuntu/ xenial main restricted universe multiverse" > /etc/apt/sources.list \
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
# http://pecl.php.net/get/redis-4.1.0.tgz                   PHP扩展
# http://download.redis.io/releases/redis-4.0.10.tar.gz     数据库
# https://cdn.mysql.com//archives/mysql-5.7/mysql-5.7.10.tar.gz
# 网络请求到后面总是失败,后面我要改成,直接把包下好在本地,dockerfile用的时候直接复制进去
# Ubuntu必备软件
    && apt-get -y install sudo vim curl wget net-tools iputils-ping git gcc make libpcre3-dev libssl-dev openssl libxslt-dev libgd-dev libgeoip-dev libcurl4-openssl-dev libmcrypt-dev libmysqlclient-dev libbz2-dev libreadline-dev libsnmp-dev libtidy-dev snmp-mibs-downloader postgresql-server-dev-all autoconf libboost-dev cmake bison libncurses5-dev\
    && bash -c '/bin/echo -e "sss\nsss\n\n\n\n\n\n\n" | adduser liuhao'\
    && chmod u+w /etc/sudoers && sed -i '/root/a liuhao  ALL=(ALL:ALL) NOPASSWD: ALL' /etc/sudoers && chmod u-w /etc/sudoers
USER liuhao
WORKDIR /home/liuhao/download
ENV APP_PATH /home/liuhao/applications
RUN whoami && pwd && bash -c '/bin/echo -e "touch nihao.php"'  \
# 下载软件(一次性下载,否则后面会下载出错)
    && wget https://cdn.mysql.com/archives/mysql-5.7/mysql-5.7.10.tar.gz\
    && wget http://nginx.org/download/nginx-1.14.0.tar.gz\
    && wget http://cn2.php.net/distributions/php-7.2.8.tar.gz\
    && wget http://pecl.php.net/get/yaf-3.0.7.tgz\
    && wget http://pecl.php.net/get/swoole-4.0.1.tgz\
    && wget http://pecl.php.net/get/redis-4.1.0.tgz\
    && wget http://download.redis.io/releases/redis-4.0.10.tar.gz\
#nginx
    # && wget http://nginx.org/download/nginx-1.14.0.tar.gz\
    && tar -zxf ./nginx-1.14.0.tar.gz && cd nginx-1.14.0/\
    && ./configure --with-cc-opt='-g -O2 -fPIE -fstack-protector-strong -Wformat -Werror=format-security -Wdate-time -D_FORTIFY_SOURCE=2' --with-ld-opt='-Wl,-Bsymbolic-functions -fPIE -pie -Wl,-z,relro -Wl,-z,now' --prefix=$APP_PATH/nginx --conf-path=$APP_PATH/nginx/etc/nginx.conf --http-log-path=log/access.log --error-log-path=log/error.log --lock-path=$APP_PATH/nginx/lock/nginx.lock --pid-path=$APP_PATH/nginx/nginx.pid --http-client-body-temp-path=$APP_PATH/nginx/body --http-fastcgi-temp-path=$APP_PATH/nginx/fastcgi --http-proxy-temp-path=$APP_PATH/nginx/proxy --http-scgi-temp-path=$APP_PATH/nginx/scgi --http-uwsgi-temp-path=$APP_PATH/nginx/uwsgi --with-debug --with-pcre-jit --with-http_ssl_module --with-http_stub_status_module --with-http_realip_module --with-http_auth_request_module --with-http_addition_module --with-http_dav_module --with-http_geoip_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_image_filter_module --with-http_v2_module --with-http_sub_module --with-http_xslt_module --with-stream --with-stream_ssl_module --with-mail --with-mail_ssl_module --with-threads\
    && make && make install\
    && sed -i '/#user  nobody;/c user  liuhao;' $APP_PATH/nginx/etc/nginx.conf\
    && sed -i "78a include ${APP_PATH}/nginx/conf.d/*.conf;" $APP_PATH/nginx/etc/nginx.conf\
    # 将这几行中的#注释全部替换掉,里面是关于PHP的server
    && sed -i '65,71s/#/ /' $APP_PATH/nginx/etc/nginx.conf\
    && sudo sed -i '/^http {/a  autoindex on;' $APP_PATH/nginx/etc/nginx.conf\
    && sudo sed -i '/^http {/a autoindex_localtime on;' $APP_PATH/nginx/etc/nginx.conf\
    && mkdir $APP_PATH/nginx/conf.d\
    && cd ../\
# PHP
    # && wget http://cn2.php.net/distributions/php-7.2.8.tar.gz\
    && tar -zxf ./php-7.2.8.tar.gz && cd php-7.2.8/\
    && ./configure --prefix=$APP_PATH/php --with-config-file-path=$APP_PATH/php/etc --enable-fpm --with-fpm-user=liuhao --with-fpm-group=liuhao --with-mysqli --with-pdo-mysql --with-iconv-dir --with-freetype-dir --with-jpeg-dir --with-png-dir --with-zlib --with-libxml-dir=/usr --enable-xml --disable-rpath --enable-bcmath --enable-shmop --enable-inline-optimization --with-curl --enable-mbregex --enable-mbstring --with-mcrypt --enable-ftp --with-gd --enable-gd-native-ttf --with-openssl --with-mhash --enable-pcntl --enable-sockets --with-xmlrpc --enable-zip --enable-soap --without-pear --with-gettext --disable-fileinfo --enable-maintainer-zts --enable-opcache --enable-exif --enable-dba  --enable-calendar --enable-sysvmsg  --enable-sysvsem --enable-sysvshm --enable-wddx --enable-phpdbg --with-bz2=/usr --with-snmp=/usr --with-tidy=/usr --with-readline=/usr --with-pdo-pgsql=/usr\
    && make && make install\
    && mkdir -p $APP_PATH/php/etc/ && cp ./php.ini-development $APP_PATH/php/etc/php.ini\
    && mv $APP_PATH/php/etc/php-fpm.conf.default $APP_PATH/php/etc/php-fpm.conf\
    && mv $APP_PATH/php/etc/php-fpm.d/www.conf.default $APP_PATH/php/etc/php-fpm.d/www.conf\
    && cd ../\
    && sudo sed -i '/^short_open_tag.*=/c short_open_tag = On' $APP_PATH/php/etc/php.ini\
    && sudo sed -i '/error_reporting.*=/c error_reporting=E_ALL' $APP_PATH/php/etc/php.ini\
    && sudo sed -i '/display_errors.*=/c display_errors=On' $APP_PATH/php/etc/php.ini\
    && sudo sed -i '/^;error_log.*=.*php_errors.log/c error_log=/var/log/php_errors.log' $APP_PATH/php/etc/php.ini\
    # redis扩展
    # && wget http://pecl.php.net/get/redis-4.1.0.tgz\
    && tar -zxf ./redis-4.1.0.tgz\
    && cd redis-4.1.0 && $APP_PATH/php/bin/phpize\
    && ./configure --with-php-config=$APP_PATH/php/bin/php-config\
    && make && make install\
    && sed -i '911a extension=redis.so' $APP_PATH/php/etc/php.ini\
    && cd ../\
    # yaf框架
	# && wget http://pecl.php.net/get/yaf-3.0.7.tgz\
	&& tar -zxf ./yaf-3.0.7.tgz\
	&& cd yaf-3.0.7 && $APP_PATH/php/bin/phpize\
	&& ./configure --with-php-config=$APP_PATH/php/bin/php-config\
	&& make && make install\
    && sed -i '911a extension=yaf.so' $APP_PATH/php/etc/php.ini\
    && cd ../\
    # swoole扩展
	# && wget http://pecl.php.net/get/swoole-4.0.1.tgz\
	&& tar -zxf ./swoole-4.0.1.tgz\
	&& cd swoole-4.0.1 && $APP_PATH/php/bin/phpize\
	&& ./configure --with-php-config=$APP_PATH/php/bin/php-config\
	&& make && make install\
	&& sed -i '911a extension=swoole.so' $APP_PATH/php/etc/php.ini\
    && cd ../\
# Redis
	# && wget http://download.redis.io/releases/redis-4.0.10.tar.gz\
	&& tar -zxf ./redis-4.0.10.tar.gz && cd redis-4.0.10\
    && make PREFIX=$APP_PATH/redis install\
    && sed -i "/dir .\//c dir $APP_PATH/redis/data" ./redis.conf\
    && mkdir -p $APP_PATH/redis/data\
    && mkdir -p $APP_PATH/redis/etc && cp ./redis.conf $APP_PATH/redis/etc/redis.conf\
    && sudo sed -i '/bind 127.0.0.1/c bind 0.0.0.0' $APP_PATH/redis/etc/redis.conf\
    && cd ../\
# Mysql5.7
    # MySQL5.7必须依赖
    && sudo apt-get -y install python-dev gccxml libbz2-dev\
    #&& wget -c http://downloads.sourceforge.net/project/boost/boost/1.59.0/boost_1_59_0.tar.gz\
    && tar -zvxf boost_1_59_0.tar.gz\
    && cd boost_1_59_0/\
    && ./bootstrap.sh\
    && ./bjam --prefix==./prefix/install\
    && ./b2 install\
    && cd ../\
    #&& wget https://cdn.mysql.com//archives/mysql-5.7/mysql-5.7.10.tar.gz >> /dev/null \
	&& tar -zxf ./mysql-5.7.10.tar.gz && cd ./mysql-5.7.10 \
    && sudo apt-get -y install cmake bison libncurses5-dev \
    && cmake \
    -DCMAKE_INSTALL_PREFIX=$APP_PATH/mysql \
    -DMYSQL_DATADIR=$APP_PATH/mysql/data \
    -DSYSCONFDIR=$APP_PATH/mysql/etc \
    -DWITH_MYISAM_STORAGE_ENGINE=1 \
    -DWITH_INNOBASE_STORAGE_ENGINE=1 \
    -DWITH_MEMORY_STORAGE_ENGINE=1 \
    -DWITH_READLINE=1 \
    -DMYSQL_UNIX_ADDR=$APP_PATH/mysql/mysql.sock \
    -DMYSQL_TCP_PORT=3306 \
    -DMYSQL_USER=mysql \
    -DENABLED_LOCAL_INFILE=1 \
    -DWITH_PARTITION_STORAGE_ENGINE=1 \
    -DEXTRA_CHARSETS=all \
    -DDEFAULT_CHARSET=utf8 \
    -DDEFAULT_COLLATION=utf8_general_ci \
    -DWITH_BOOST=../boost_1_59_0\
    # 这里如果make -j4启动4个gcc会导致内存消耗过大,产生make[2]: *** [libmysqld/CMakeFiles/sql_embedded.dir/__/sql/item_geofunc.cc.o] Error 4 错误
    && make && make install\
    # 这里提示目录不存在
    #&& sudo sed -i '/bind-address/c bind-address		= 0.0.0.0' $APP_PATH/mysql/etc/conf.d/mysql.cnf