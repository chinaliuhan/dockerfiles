FROM ubuntu:16.04
USER root
WORKDIR /root
# Ubuntu必备操作,这里不用使用-e也会换行...... 用了-e 反而报错,奇迹
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
# Ubuntu必备软件 
    && apt-get -y install curl vim \
    && apt-get -y install net-tools \
    && apt-get -y install iputils-ping \
# 钱包依赖
    && apt-get -y install wget sudo git \
# 常用软件 
    && apt-get -y install  shadowsocks \
    #&& `nohup sslocal -s www.yxbang.cn  -p 7002  -b 127.0.0.1 -l 1080 -k '!(password)' -m aes-256-cfb >> ~/sslocal.log >> /dev/null&` \
    #&& export ALL_PROXY=socks5://127.0.0.1:1080 \ 
    #&& git config --global http.proxy 'socks5://127.0.0.1:1080' \
    #&& git config --global https.proxy 'socks5://127.0.0.1:1080' \
# Achain钱包依赖
    && apt-get -y install cmake git libreadline-dev uuid-dev g++ libncurses5-dev zip libssl-dev openssl pkg-config build-essential python-dev autoconf autotools-dev libicu-dev libbz2-dev libboost-dev libboost-all-dev \
    && apt-get -y install ntp \
    && apt-get -y install ntpdate \
    && git clone https://github.com/Achain-Dev/Achain.git \
    # Achain需要授时
    && echo "service ntp stop \n ntpdate -s time.nist.gov \n  service ntp start" > date.sh
    # 注意这里如果不是用ROOT用户启动的话,需要用sudo,文档没有指出,但是在运行有一步命令的时候需要用的sudo,具体我忘了
    # 而且前面安装sudo 还有一个好处,如果你想新建多个用户的话,也会很方便echo 'liuhao  ALL=(ALL:ALL) ALL' >> /etc/sudoers
    # 好像是leveldb-1.20需要,主要如果上面不安装sudo 的话这里也会报错
    # 因为钱包编译文件里有sudo 命令,除非你愿意手动删除,否则就要apt-get instal xxx 
    # 注意wget也是必须的,有些文件他是用wget下载还有git
CMD ./date.sh && cd /root/Achain/ && ./build_ubuntu.sh download
