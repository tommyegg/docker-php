FROM php:8.3-fpm

RUN groupmod -g 1000 www-data
RUN useradd -u 1000 -ms /bin/bash -g www-data laravel

RUN apt-get update && apt-get install -y libzip-dev zip libfreetype6-dev libjpeg62-turbo-dev libmcrypt-dev libpng-dev cron supervisor libicu-dev nodejs npm\
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-configure zip \
    && docker-php-ext-install gd mysqli pdo_mysql zip opcache bcmath iconv intl

RUN pecl install redis-6.0.2 \
        && pecl install mcrypt-1.0.7 \
    && docker-php-ext-enable redis mcrypt

#設定supervisor
RUN mkdir -p /var/log/supervisor
RUN mkdir -p /www
WORKDIR /www
RUN chown -R laravel:www-data /www

COPY ./crontab /var/spool/cron/crontabs/root
#設定crontab
RUN chown -R root:crontab /var/spool/cron/crontabs/root \
    && chmod 600 /var/spool/cron/crontabs/root \
    && touch /var/log/cron.log \
    && crontab /var/spool/cron/crontabs/root

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

CMD ["/usr/bin/supervisord"]
