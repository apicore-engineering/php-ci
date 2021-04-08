FROM php:7.4-buster
LABEL maintainer="bence.gazder@apicore.de"

# install dependencies
RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini" && \
    apt-get update && apt-get install -y \
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
    libonig-dev \
    zip \
    libzip-dev \
    $PHPIZE_DEPS \
    sudo \
# configure extensions
    && docker-php-ext-configure intl --enable-intl \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
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
    && pecl install redis \
    && docker-php-ext-enable redis \
# cleanup
    && docker-php-source delete \
    && apt-get purge -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libcurl4-openssl-dev \
    libpng-dev \
    libicu-dev \
    $PHPIZE_DEPS \
    && apt-get clean -y \
    && apt-get autoremove -y \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /usr/src/ \
    && rm -rf /usr/local/lib/php/test/ \
    && rm -rf /usr/local/bin/phpdbg \
    && echo "Installed packages:" \
    && dpkg --list \
    && php -i \
    && php -v

CMD ["php", "-v"]
