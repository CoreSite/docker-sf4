FROM php:7.1.20-fpm

VOLUME ["/var/www/html"]

RUN apt-get update && apt-get install -y \
        sudo \
        zlib1g-dev \
        libicu-dev \
        libpq-dev \
        git \
        vim \
        zip \
        wget \
        mc \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        gnupg2 \
        htop \
        nano \
        mysql-client

RUN docker-php-ext-install -j$(nproc) iconv mcrypt \
    && docker-php-ext-install -j$(nproc) pdo pdo_mysql mysqli \
    && docker-php-ext-configure bcmath \
    && docker-php-ext-install -j$(nproc) bcmath \
    && docker-php-ext-configure mbstring \
    && docker-php-ext-install -j$(nproc) mbstring \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install -j$(nproc) intl zip opcache pcntl \
    && docker-php-ext-configure calendar --enable-calendar \
    && docker-php-ext-install -j$(nproc) calendar

RUN echo America/New_York > /etc/timezone && \
    dpkg-reconfigure -f noninteractive tzdata

# install node.js & yarn
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash -
RUN apt-get install -y nodejs
RUN curl -sS http://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb http://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update && apt-get install -y yarn

# install composer
RUN set -x && curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer

# prepare permissions
RUN usermod -u 33 www-data
RUN groupmod -g 33 www-data
RUN chsh -s /bin/bash www-data
RUN chown -R www-data:www-data /var/www

# ADD php.ini /usr/local/etc/php/conf.d/40-custom.ini

WORKDIR /var/www/html

CMD ["php-fpm"]

EXPOSE 80
EXPOSE 443
