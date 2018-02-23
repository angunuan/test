FROM php:5.6-fpm-jessie

MAINTAINER houzi <houzi@beihai365.com>

# 设置时区
RUN echo "Asia/Shanghai" > /etc/timezone

# 添加php.ini
COPY config/php.ini-development /usr/local/etc/php/php.ini

# 安装依赖
#   1.更新源
#   2.安装依赖，自动应答，不安装推荐软件包
# 
# 安装PHP扩展
#   1.通过 docker-php-ext-configure 配置扩展
#   2.通过 docker-php-ext-install 安装扩展
#   3.通过 pecl 安装扩展
# 
# 清理
#   1.删除通过apt安装所下载的包
#   2.清空缓存目录
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        libmcrypt-dev \
        zlib1g-dev \
        libmemcached-dev \
        libgearman-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng12-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) \
        mcrypt \
        mysql \
        mysqli \
        pdo_mysql \
        gd \
    && pecl install xdebug \
    && pecl install memcache \
    && pecl install memcached-2.2.0 \
    && pecl install redis \
    && pecl install gearman \
    && docker-php-ext-enable xdebug memcache memcached redis gearman \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
