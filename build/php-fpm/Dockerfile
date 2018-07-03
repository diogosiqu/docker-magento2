FROM php:7.0-fpm

MAINTAINER Gustavo Bacelar <gubacelar@gmail.com>

WORKDIR /usr/share/nginx/html
# Install dependencies
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libcurl4-nss-dev \
        libc-client-dev \
        libkrb5-dev \
        firebird-dev \
        libicu-dev \
        libxml2-dev \
        libxslt1-dev \
        ssmtp \
        libssh2-1-dev \
    && docker-php-ext-install -j$(nproc) mcrypt \
    && docker-php-ext-install -j$(nproc) bcmath \
    && docker-php-ext-install -j$(nproc) curl \
    && docker-php-ext-install -j$(nproc) mbstring \
    && docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-install -j$(nproc) interbase \
    && docker-php-ext-install -j$(nproc) intl \
    && docker-php-ext-install -j$(nproc) soap \
    && docker-php-ext-install -j$(nproc) xmlrpc \
    && docker-php-ext-install -j$(nproc) xsl \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install imap \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install zip


# # Install the xdebug extension
# RUN pecl install xdebug
# RUN docker-php-ext-enable xdebug


# Copy xdebug configration for remote debugging
COPY files/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

COPY files/ioncube_loader_lin_7.0.so /usr/local/lib/php/extensions/no-debug-non-zts-20131226/ioncube_loader_lin_7.0.so

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer

RUN apt-get -y autoclean && apt-get -y autoremove && apt-get -y clean && rm -rf /var/lib/apt/lists/* && apt-get update

# Expose ports
EXPOSE 9000

# Set the default command to execute
# when creating a new container
CMD php-fpm
