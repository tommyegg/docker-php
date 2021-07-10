FROM php:8.0.7-fpm

RUN apt-get update && apt-get install -y libzip-dev zip libfreetype6-dev libjpeg62-turbo-dev libmcrypt-dev libpng-dev cron supervisor \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-configure zip \
    && docker-php-ext-install gd mysqli pdo_mysql zip opcache bcmath iconv

RUN pecl install redis-5.3.4 \
	&& pecl install mcrypt-1.0.4 \
	&& pecl install swoole-4.5.9 \
	&& pecl install inotify-3.0.0 \
    && docker-php-ext-enable redis swoole inotify mcrypt

#設定supervisor
RUN mkdir -p /var/log/supervisor
RUN mkdir -p /www
WORKDIR /www
RUN chown -R www-data:www-data /www

COPY ./crontab /var/spool/cron/crontabs/root
#設定crontab
RUN chown -R root:crontab /var/spool/cron/crontabs/root \
    && chmod 600 /var/spool/cron/crontabs/root \
    && touch /var/log/cron.log \
    && crontab /var/spool/cron/crontabs/root

CMD ["/usr/bin/supervisord"]