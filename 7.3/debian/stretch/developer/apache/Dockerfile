FROM php:7.3-apache-stretch
LABEL maintainer="bence.gazder@apicore.de"

ENV COMPOSER_VERSION 1.9.1

# install dependencies
RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini" && \ 
    apt update && apt install -y \
    supervisor \
    cron \
    ca-certificates \
    libfreetype6 \
    libfreetype6-dev \
    libssl1.0 \
    libpng16-16 \
    libjpeg62-turbo \
    libjpeg62-turbo-dev \
    mcrypt \
    libcurl4-openssl-dev \
    libpng-dev \
    libicu-dev \
    zip \
    libzip-dev \
    $PHPIZE_DEPS \
    nano \
    git \
    openssh-client \
    sudo \
# configure extensions
    && docker-php-ext-configure intl --enable-intl \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
# install extensions
    && docker-php-ext-install \
    mbstring \
    intl \
    curl \
    json \
    pdo_mysql \
    exif \
    zip \
    gd \
    bcmath \
    opcache \
    pcntl \
    sockets \
# pecl extensions
    && pecl install xdebug \
    && docker-php-ext-enable xdebug \
# composer
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer --version=${COMPOSER_VERSION} \
    && chmod +x /usr/local/bin/composer \
# set up composer
    && composer global require hirak/prestissimo --profile --prefer-dist --no-progress --no-suggest --optimize-autoloader --no-interaction \
# configure stuff
    && a2enmod rewrite \
    && echo "ServerName localhost" >> /etc/apache2/apache2.conf \
# cleanup
    && docker-php-source delete \
    && apt purge -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libcurl4-openssl-dev \
    libpng-dev \
    libicu-dev \
    $PHPIZE_DEPS \
    && apt autoclean -y \
    && apt autoremove -y \
    && echo "Installed packages:" \
    && dpkg --list \
    && php -i \
    && php -v

CMD ["php", "-v"]
