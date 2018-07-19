FROM ubuntu:16.04
RUN  echo "deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse"  > /etc/apt/sources.list \
    && echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse" >> /etc/apt/sources.list \
    && echo "deb-src http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse" >> /etc/apt/sources.list \
    && apt-get -y clean && apt-get -y autoclean \
    && apt-get -y update \
    && apt-get -y install sudo\
    && bash -c '/bin/echo -e "sss\nsss\n\n\n\n\ny\n" | adduser liuhao'\
    && chmod u+w /etc/sudoers && sed -i '/root/a liuhao  ALL=(ALL:ALL) NOPASSWD: ALL' /etc/sudoers && chmod u-w /etc/sudoers
USER liuhao
WORKDIR /home/liuhao
RUN sudo apt-get install -y git vim curl wget net-tools iputils-ping\
# Nginx 配置文件路径/etc/nginx/conf.d
 	&& sudo apt-get -y install nginx\
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
# Redis因为高版本才能读取低版本的dump.rdb文件
	&& sudo apt-get -y install redis-server\
# Mysql5.7
    && echo "mysql-server-5.5 mysql-server/root_password password sss" | debconf-set-selections\
    && echo "mysql-server-5.5 mysql-server/root_password_again password sss" | debconf-set-selections\
	&& sudo apt-get -y install mysql-server-5.7\
# 清空下载
    && sudo apt-get -y clean && sudo apt-get -y autoclean\
# 修改配置文件
    && sudo sed -i '/^short_open_tag.*=/c short_open_tag = On' /etc/php/7.0/fpm/php.ini\
    && sudo sed -i '/^short_open_tag.*=/c short_open_tag = On' /etc/php/7.0/cli/php.ini\
    && sudo sed -i '/error_reporting.*=/c error_reporting=E_ALL' /etc/php/7.0/fpm/php.ini\
    && sudo sed -i '/error_reporting.*=/c error_reporting=E_ALL' /etc/php/7.0/cli/php.ini\
    && sudo sed -i '/display_errors.*=/c display_errors=On' /etc/php/7.0/fpm/php.ini\
    && sudo sed -i '/display_errors.*=/c display_errors=On' /etc/php/7.0/cli/php.ini\
    && sudo sed -i '/^;error_log.*=.*php_errors.log/c error_log=/var/log/php_errors.log' /etc/php/7.0/fpm/php.ini\
    && sudo sed -i '/^;error_log.*=.*php_errors.log/c error_log=/var/log/php_errors.log' /etc/php/7.0/cli/php.ini\
    && sudo sed -i '/[^#]root.*www/c root /home/liuhao/www;' /etc/nginx/sites-available/default\
    && sudo sed -i '21a autoindex on;' /etc/nginx/nginx.conf\
    && sudo sed -i '21a autoindex_localtime on;' /etc/nginx/nginx.conf\
    && sudo sed -i '/bind 127.0.0.1/c bind 0.0.0.0' /etc/redis/redis.conf\
    && sudo sed -i '/bind-address/c bind-address		= 0.0.0.0' /etc/mysql/mysql.conf.d/mysqld.cnf\
# 准备脚本文件
    && echo "#! /bin/bash" >> $HOME/init.sh \
    && echo "sudo service nginx start" >> $HOME/init.sh \
    && echo "sudo service mysql start" >> $HOME/init.sh \
    && echo "sudo service redis-server start" >> $HOME/init.sh \
    && echo "sudo service php7.0-fpm start" >> $HOME/init.sh \
    # 指定MySQL挂载目录, 这个版本的MySQL有这个问题,要不然会无法启动,提示,如果目录不存在需要加上-s命令
    && echo "sudo usermod -d /var/lib/mysql/ mysql" >> $HOME/init.sh \
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
    && echo "sudo service php7.0-fpm stop" >> $HOME/stop.sh\
CMD $HOME/init.sh