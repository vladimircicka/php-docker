FROM php:7.4-fpm

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
    
RUN docker-php-ext-install \
    bcmath \
    bz2 \
    calendar \
    ctype \
    curl \
    dba \
    dom \
    exif \
    fileinfo \
    ftp \
    iconv \
    intl \
    mbstring \
    mysqli \
    opcache \
    pcntl \
    pdo \
    pdo_mysql \
    phar \
    posix \
    simplexml \
    soap \
    xsl \
    zip

RUN docker-php-ext-enable xsl mysqli curl dom fileinfo gd iconv intl exif pdo_mysql mbstring simplexml zip

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

