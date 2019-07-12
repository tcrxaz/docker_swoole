FROM php:fpm-alpine

LABEL maintainer="Lance Chen <tcrxaz@gmail.com>"

ARG SWOOLE_VERSION
ARG TIMEZONE

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories 

# init
RUN apk --update --no-progress --no-cache add shadow \
    freetype-dev \
    libjpeg-turbo-dev \
    libpng-dev \
    libmcrypt-dev \
    git \
    zip \
    unzip \
    openssl-dev \
    wget \
    curl \
    m4 \
    autoconf \
    make \
    gcc \
    g++ 

# install php extentions redis
RUN docker-php-ext-configure gd \
        --with-gd \
        --with-freetype-dir=/usr/include/ \
        --with-png-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ \
    &&  docker-php-ext-install \
                    fileinfo \
                    pdo_mysql \
                    mysqli \
                    gd \
    && pecl install redis \
    && docker-php-ext-enable redis opcache

# install swoole
RUN curl -# -L https://github.com/swoole/swoole-src/archive/v${SWOOLE_VERSION}.zip -o swoole.zip \
    && unzip swoole.zip \
    && rm swoole.zip \
    && ( \
        cd swoole-src-${SWOOLE_VERSION} \
        && phpize \
        && ./configure --enable-mysqlnd --enable-openssl --enable-http2 \
        && make -j \
        && make install \
    ) \
    && rm -rf swoole-src-${SWOOLE_VERSION} \
    && docker-php-ext-enable swoole

# install composer
RUN curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && composer self-update --clean-backups

# set timezone and authorization user
RUN apk add --no-cache tzdata \
    && cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
    && echo "${TIMEZONE}" > /etc/timezone \
    && apk del tzdata \
    && usermod -u 1000 www-data

EXPOSE 9000
CMD ["php-fpm"]