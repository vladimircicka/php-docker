FROM php:8.1-fpm

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
        curl \
        openssl \
        libssl-dev \
        libcurl4-openssl-dev \
        libxml2-dev \
        libwebp-dev \
        libjpeg-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libxpm-dev \
        libfreetype6-dev \
        zlib1g-dev \
        libzip-dev \
        libonig-dev \
        libxslt1-dev \
        libldap2-dev \
        libbz2-dev \
        libz-dev \
        libmemcached-dev && \
    rm -r /var/lib/apt/lists/*

RUN docker-php-source extract

RUN docker-php-ext-configure gd \
    --with-freetype \
	--with-jpeg \
	--with-xpm \
	--with-webp \
    --enable-gd \
	&& docker-php-ext-install -j$(nproc) gd 
    
RUN docker-php-ext-install bcmath
RUN docker-php-ext-install bz2
RUN docker-php-ext-install calendar
RUN docker-php-ext-install ctype
RUN docker-php-ext-install curl
RUN docker-php-ext-install dba
RUN docker-php-ext-install dom
RUN docker-php-ext-install exif
RUN docker-php-ext-install fileinfo
RUN docker-php-ext-install ftp
RUN docker-php-ext-install intl
RUN docker-php-ext-install mbstring
RUN docker-php-ext-install mysqli
RUN docker-php-ext-install opcache
RUN docker-php-ext-install pcntl
RUN docker-php-ext-install pdo
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install phar
RUN docker-php-ext-install posix
RUN docker-php-ext-install simplexml
RUN docker-php-ext-install xsl
RUN docker-php-ext-install zip

RUN docker-php-ext-enable xsl mysqli curl dom fileinfo gd intl exif pdo_mysql mbstring simplexml zip

RUN pecl install memcached && \
    echo extension=memcached.so >> /usr/local/etc/php/conf.d/memcached.ini && \
    pecl install -f xdebug-3.1.0 && \
    docker-php-ext-enable xdebug

# Cleanup docker files
RUN apt-get remove -y git && \
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    docker-php-source delete

ARG USER_ID=1000
ARG GROUP_ID=1000

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN curl -sL https://squizlabs.github.io/PHP_CodeSniffer/phpcs.phar -o /usr/bin/phpcs \
	&& chmod a+x /usr/bin/phpcs

RUN usermod -u $GROUP_ID www-data

