FROM php:7.4-fpm

# 更换 Debian 镜像源（使用阿里云镜像）
RUN sed -i 's/deb.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list && \
    sed -i 's/security.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list

# 安装系统依赖
RUN apt-get update && \
    apt-get install -y --fix-missing wget vim git curl telnet unzip zlib1g-dev \
    libpng-dev libjpeg-dev libfreetype6-dev libzip-dev libmemcached-dev libgmp-dev \
    nginx supervisor

# 安装 PHP 扩展
RUN docker-php-ext-install gd zip pdo pdo_mysql opcache mysqli && \
    pecl install redis memcached && \
    docker-php-ext-enable redis memcached && \
    docker-php-ext-configure gmp && \
    docker-php-ext-install gmp

# 安装 Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer && \
    php -r "unlink('composer-setup.php');" && \
    chmod +x /usr/local/bin/composer && \
    composer --version

# 清理
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# 设置工作目录
WORKDIR /home/www
