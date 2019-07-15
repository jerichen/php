FROM php:7.2-fpm

MAINTAINER JeriChen<jeri.chen0110@gmail.com>

ENV TZ=Asia/Taipei
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Get packages that we need in container
RUN apt-get update -y && \
    apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    git \
    vim

# Install recommended extensions for Symfony
RUN apt-get update && apt-get install -y \
        libicu-dev \
    && docker-php-ext-install \
        intl \
        opcache \
    && docker-php-ext-enable \
        intl \
        opcache \
    && docker-php-ext-install pdo pdo_mysql \
    && docker-php-ext-install mysqli

# Needed for the php extensions we enable below
RUN apt-get update && apt-get install -y \
    zip \
    unzip \
    zlib1g-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libwebp-dev \
    libxml2-dev \
    libxslt-dev

RUN docker-php-ext-install -j$(nproc) iconv && \
    docker-php-ext-configure gd \
        --with-gd \
        --with-png-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-configure xml \
        --with-libxml-dir=/usr/include/ && \
    docker-php-ext-configure xsl \
        --with-libxslt-dir=/usr/include/ && \
    docker-php-ext-configure zip \
        --with-zlib-dir=/usr && \
    docker-php-ext-install -j$(nproc) gd

# clear
RUN rm -r /var/lib/apt/lists

# Install phpunit
RUN curl https://phar.phpunit.de/phpunit.phar -L > phpunit.phar && \
      chmod +x phpunit.phar && \
      mv phpunit.phar /usr/local/bin/phpunit && \
      phpunit --version

COPY ./ /var/www/html/
WORKDIR /var/www/html
