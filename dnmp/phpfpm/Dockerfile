# 从官方基础版本构建,定制一个比较通用的镜像
FROM php:7.4-fpm

# 时区指定为 Asia/Shanghai, 默认为: Etc/UTC
ENV TIMEZONE Asia/Shanghai

# 配置系统时区为 Asia/Shangh 时区
RUN ln -snf /usr/share/zoneinfo/$TIMEZONE /etc/localtime && echo $TIMEZONE > /etc/timezone

# docker中php扩展安装方式
# 1、PHP源码文件目录自带扩展 docker-php-ext-install直接安装
# 2、pecl扩展 因为一些扩展不包含在PHP源码文件中，PHP 的扩展库仓库中存在。用 pecl install 安装扩展，再用   docker-php-ext-enable 命令 启用扩展
# 3、其他扩展 一些既不在 PHP 源码包，也不再 PECL 扩展仓库中的扩展，可以通过下载扩展程序源码，编译安装的方式安装

# 1、安装自带的扩展： 

# a、不需要配置参数的
# ./docker-php-ext-install pdo_mysql mysqli sockets gettext opcache

# b、需要配置参数的话，以gd扩展为例
# 如果不配置参数直接安装
# ./docker-php-ext-install gd

# 扩展版本号定义
# redis
ENV PHPREDIS_VERSION=5.3.0

# 增加核心扩展: bcmath, sockets, mysqli, pcntl, pdo_mysql
# Mysqli 扩展 自带 直接安装即可(当前数据库使用的mysqli查询的)
# PDO 扩展 自带 直接安装即可
# Bcmath 扩展 自带 直接安装即可
RUN docker-php-ext-install -j$(nproc) bcmath sockets mysqli pcntl pdo_mysql
# 增加 opcache 扩展 
RUN docker-php-ext-configure opcache --enable-opcache && docker-php-ext-install opcache

# 扩展依赖
RUN apt-get update \
    && apt-get install -y \
        curl \
        wget \
        git \
        zip \
        libz-dev \
        libssl-dev \
        libnghttp2-dev \
        libpcre3-dev \
        libmemcached-dev \
        zlib1g-dev \
    && apt-get clean \
    && apt-get autoremove -y

#for gd
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
    && docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

#for redis 
RUN curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/$PHPREDIS_VERSION.tar.gz \
    && tar xfz /tmp/redis.tar.gz \
    && rm -r /tmp/redis.tar.gz \
    && mkdir -p /usr/src/php/ext \
    && mv phpredis-$PHPREDIS_VERSION /usr/src/php/ext/redis \
    && docker-php-ext-install redis 

# 添加配置文件, /usr/local/etc/php/php.ini
#RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"
 RUN cp "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

LABEL Author="Liu"
LABEL Version="1.0-fpm"
LABEL Description="PHP FPM 7.4 镜像. with extensions."
