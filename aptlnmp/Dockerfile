FROM ubuntu:16.04
RUN apt-get -y clean\
    && echo "deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse"  > /etc/apt/sources.list \
    && echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse" >> /etc/apt/sources.list \
    && apt-get -y update \
    && apt-get -y install sudo git vim curl wget net-tools iputils-ping\
    ## 添加nginx官方给的最新稳定版的源,wget这个是证书   
    && wget https://nginx.org/keys/nginx_signing.key\
    && apt-key add nginx_signing.key && rm nginx_signing*\
    && echo "deb http://nginx.org/packages/ubuntu/ xenial nginx" >> /etc/apt/sources.list\
    && echo "deb-src http://nginx.org/packages/ubuntu/ xenial nginx" >> /etc/apt/sources.list\
    && apt-get update\
    # 新建普通用户
    && bash -c '/bin/echo -e "sss\nsss\n\n\n\n\ny\n" | adduser liuhao'\
    && chmod u+w /etc/sudoers && sed -i '/root/a liuhao  ALL=(ALL:ALL) NOPASSWD: ALL' /etc/sudoers && chmod u-w /etc/sudoers
USER liuhao
WORKDIR /home/liuhao
RUN echo ":set nu\n:set ts=4\n:set confirm\n:syntax on\n:set showmatch\n:set encoding=utf-8\n:set shiftwidth=4\n:set ai\n:set ruler\n:set helplang=cn\n:set hlsearch\n:set cursorline\n:set laststatus=2" > $HOME/.vimrc\
# Nginx 配置文件路径/etc/nginx/conf.d
 	&& sudo apt-get -y install nginx=1.14.0-1~xenial\
    && sudo sed -i '/^http/a autoindex_localtime on;' /etc/nginx/nginx.conf\
    && sudo sed -i '/^http/a autoindex on;' /etc/nginx/nginx.conf\
    && sudo sed -i '/^user\s\+\w\+;/c user liuhao;' /etc/nginx/nginx.conf\
    #nginx基础配置文件Localhost
    && sudo bash -c "echo 'server {' > /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo 'listen 80;' >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo 'server_name lh.localhost;' >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo -e \"\n\" >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo 'access_log  /var/log/nginx/access_locahost_log;' >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo 'error_log   /var/log/nginx/error_locahost_log;' >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo -e \"\n\" >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo 'index index.html index.htm index.php index.phtml;' >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo 'client_max_body_size 100m;' >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo -e \"\n\" >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo 'set \$public_root /home/liuhao/www;' >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo 'root \$public_root;' >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo -e \"\n\" >> /etc/nginx/conf.d/lh.localhost.conf"\
    # 这里的!无法屏蔽,使用会被当做系统变量用或者存在和\!输出.所以干脆输出了,后面在用sed替换掉
    && sudo bash -c "echo 'if (\!-e \$request_filename) {' >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo 'rewrite ^/(.*)  /index.php?/\$1 last;' >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo '}' >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo -e \"\n\" >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo 'location / {' >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo 'try_files \$uri \$uri/ \$uri=404;' >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo '}' >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo -e \"\n\" >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo 'location ~ \.php\$ {' >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo 'fastcgi_pass unix:/run/php/php7.0-fpm.sock;' >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo 'fastcgi_index  index.php;' >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo 'include fastcgi_params;' >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo 'fastcgi_param  SCRIPT_FILENAME  \$document_root\$fastcgi_script_name;' >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo '}' >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo bash -c "echo '}' >> /etc/nginx/conf.d/lh.localhost.conf"\
    && sudo sed -i 's/\\!-e/!-e/' /etc/nginx/conf.d/lh.localhost.conf\
# PHP环境软件 配置文件路径/etc/php/7.0/cli/php.ini /etc/php/7.0/fpm/php.ini /etc/php/7.0/fpm/php-fpm.conf
	&& sudo apt-get -y install php7.0 php7.0-curl php7.0-gd php7.0-dev php7.0-mbstring php7.0-opcache php7.0-zip php7.0-mysql\
	&& wget http://pecl.php.net/get/yaf-3.0.7.tgz\
	&& wget http://pecl.php.net/get/swoole-4.0.1.tgz\
    && wget http://pecl.php.net/get/redis-4.1.0.tgz\
    && tar -zxf ./redis-4.1.0.tgz\
    && cd redis-4.1.0 && phpize\
    && ./configure --with-php-config=/usr/bin/php-config\
    && make && sudo make install\
    && sudo bash -c "echo 'extension=redis.so' > /etc/php/7.0/mods-available/redis.ini"\
	&& sudo ln -s /etc/php/7.0/mods-available/redis.ini /etc/php/7.0/cli/conf.d/\
	&& sudo ln -s /etc/php/7.0/mods-available/redis.ini /etc/php/7.0/fpm/conf.d/\
    && cd ../\
    && rm -rf ./redis*\
	&& tar -zxf ./yaf-3.0.7.tgz\
	&& cd yaf-3.0.7 && phpize\
	&& ./configure --with-php-config=/usr/bin/php-config\
	&& make && sudo make install\
    && sudo bash -c "echo 'extension=yaf.so' > /etc/php/7.0/mods-available/yaf.ini"\
	&& sudo ln -s /etc/php/7.0/mods-available/yaf.ini /etc/php/7.0/cli/conf.d/\
	&& sudo ln -s /etc/php/7.0/mods-available/yaf.ini /etc/php/7.0/fpm/conf.d/\
    && cd ../\
    && rm -rf ./yaf*\
	&& tar -zxf ./swoole-4.0.1.tgz\
	&& cd swoole-4.0.1 && phpize\
	&& ./configure --with-php-config=/usr/bin/php-config\
	&& make && sudo make install\
    && sudo bash -c "echo 'extension=swoole.so' > /etc/php/7.0/mods-available/swoole.ini"\
	&& sudo ln -s /etc/php/7.0/mods-available/swoole.ini /etc/php/7.0/cli/conf.d/\
	&& sudo ln -s /etc/php/7.0/mods-available/swoole.ini /etc/php/7.0/fpm/conf.d/\
    && cd ../\
    && rm -rf ./swoole*\
    && sudo sed -i '/^short_open_tag.*=/c short_open_tag = On' /etc/php/7.0/fpm/php.ini\
    && sudo sed -i '/^short_open_tag.*=/c short_open_tag = On' /etc/php/7.0/cli/php.ini\
    && sudo sed -i '/error_reporting.*=/c error_reporting=E_ALL' /etc/php/7.0/fpm/php.ini\
    && sudo sed -i '/error_reporting.*=/c error_reporting=E_ALL' /etc/php/7.0/cli/php.ini\
    && sudo sed -i '/display_errors.*=/c display_errors=On' /etc/php/7.0/fpm/php.ini\
    && sudo sed -i '/display_errors.*=/c display_errors=On' /etc/php/7.0/cli/php.ini\
    && sudo sed -i '/^;error_log.*=.*php_errors.log/c error_log=/var/log/php_errors.log' /etc/php/7.0/fpm/php.ini\
    && sudo sed -i '/^;error_log.*=.*php_errors.log/c error_log=/var/log/php_errors.log' /etc/php/7.0/cli/php.ini\
    && sudo sed -i '/^user\s=\s\w\+/c user = liuhao' /etc/php/7.0/fpm/pool.d/www.conf\
    && sudo sed -i '/^group\s=\s\w\+/c group = liuhao' /etc/php/7.0/fpm/pool.d/www.conf\
# Redis因为高版本才能读取低版本的dump.rdb文件
	&& sudo apt-get -y install redis-server\
    && sudo sed -i '/bind 127.0.0.1/c bind 0.0.0.0' /etc/redis/redis.conf\
# Mysql5.7
    && echo "mysql-server-5.5 mysql-server/root_password password sss" | sudo debconf-set-selections\
    && echo "mysql-server-5.5 mysql-server/root_password_again password sss" | sudo debconf-set-selections\
	&& sudo apt-get -y install mysql-server-5.7\
    && sudo sed -i '/bind-address/c bind-address		= 0.0.0.0' /etc/mysql/mysql.conf.d/mysqld.cnf\
    # 指定MySQL挂载目录, 这个版本的MySQL有这个问题,要不然会无法启动,提示,如果目录不存在需要加上-s命令
    && sudo usermod -d /var/lib/mysql/ mysql\
    # 其实权限是已经有的, 但是就是不要用必须要在运行一次, 而且直接在dockerfile中也不行,必须在系统中运行,,,所以我放到了下面的init.sh中
    # 如果不给权限的话,运行mysql的时候会提示没有user表.没有private表...之类的
    # && sudo mkdir /var/run/mysqld && sudo chown -R mysql:mysql /var/lib/mysql /var/run/mysqld\
# 清空下载
    && sudo apt-get -y clean\
# 准备脚本文件
    && echo "#! /bin/bash" >> $HOME/init.sh \
    && echo "sudo service nginx start" >> $HOME/init.sh \
    && echo "sudo service mysql start" >> $HOME/init.sh \
    && echo "sudo service redis-server start" >> $HOME/init.sh \
    && echo "sudo service php7.0-fpm start" >> $HOME/init.sh \
    && echo "sudo chown -R mysql:mysql /var/lib/mysql /var/run/mysqld" >> $HOME/init.sh\
    && echo 'echo "UPDATE mysql.user SET host=\"%\" where user=\"root\"; flush privileges;" | mysql -uroot -psss' >> $HOME/init.sh \
    && echo "#! /bin/bash" >> $HOME/restart.sh\
    && echo "sudo service nginx restart" >> $HOME/restart.sh\
    && echo "sudo service mysql restart" >> $HOME/restart.sh\
    && echo "sudo service redis-server restart" >> $HOME/restart.sh\
    && echo "sudo service php7.0-fpm restart" >> $HOME/restart.sh\
    && echo "#! /bin/bash" >> $HOME/stop.sh\
    && echo "sudo service nginx stop" >> $HOME/stop.sh\
    && echo "sudo service mysql stop" >> $HOME/stop.sh\
    && echo "sudo service redis-server stop" >> $HOME/stop.sh\
    && echo "sudo service php7.0-fpm stop" >> $HOME/stop.sh