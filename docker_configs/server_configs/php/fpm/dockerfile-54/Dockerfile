FROM php:5.4-fpm

MAINTAINER Kazarmin Dmitriy <phoenix1991@yandex.ru>

ADD ./debian_jessie_sources_list /etc/apt/sources.list

RUN apt-get update && apt-get install -y \
    libbz2-dev \
    libpq-dev \
    libmemcached-dev \
    memcached \
    curl \
    wget \
    git \
    ssmtp \
    nano \
    libjpeg-dev \
    libxslt-dev \
    libpng12-dev \
    libfreetype6-dev \
    freetype* \
    libssl-dev \
    libmcrypt-dev \
    libldap2-dev \
    mysql-client \
    libxml2-dev \
    --no-install-recommends \
    && rm -r /var/lib/apt/lists/*

# configure gd library
RUN docker-php-ext-configure gd \
    --enable-gd-native-ttf \
    --with-jpeg-dir=/usr/lib \
    --with-freetype-dir=/usr/include/freetype2 && \
    docker-php-ext-install gd

RUN ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/include/gmp.h && \
    ln -fs /usr/lib/x86_64-linux-gnu/libldap.so /usr/lib/ && \
    docker-php-ext-configure ldap \
        --with-libdir=lib/x86_64-linux-gnu

RUN curl -s http://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer

RUN docker-php-ext-install \
    exif \
    bz2 \
    gettext \
    calendar \
    mbstring \
    zip \
    xsl \
    ldap \
    soap \
    pdo \
    pdo_mysql \
    mysql \
    mysqli \
    sockets \
    mcrypt

RUN pecl install \
    timezonedb \
    memcache \
    igbinary-2.0.8 \
    zendopcache \
    && docker-php-ext-enable \
    timezonedb \
    memcache \
    igbinary \
    opcache

ARG SITENAME=!domain!
ADD ./site.ini /usr/local/etc/php/conf.d
ADD ./site.pool.conf /usr/local/etc/php-fpm.d/
#COPY ./ssmtp/* /etc/ssmtp/

RUN usermod -u 1000 www-data

WORKDIR /var/www/html

CMD ["php-fpm"]

EXPOSE 9000