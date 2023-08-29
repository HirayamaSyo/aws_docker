FROM php:8.1-fpm-bullseye

ARG project

ENV TZ=Asia/Tokyo \
  # locale
  LANG=ja_JP.UTF-8 \
  LANGUAGE=ja_JP:ja \
  LC_ALL=ja_JP.UTF-8 \
  # composer environment
  COMPOSER_ALLOW_SUPERUSER=1 \
  COMPOSER_HOME=/composer

COPY --from=composer:2.3 /usr/bin/composer /usr/bin/composer

RUN apt-get update && \
	apt-get install -y git unzip libzip-dev libicu-dev libonig-dev nodejs npm libfreetype6-dev libjpeg62-turbo-dev libpng-dev && \
    docker-php-ext-install intl pdo_mysql zip bcmath gd

COPY ./$project /var/www/html
COPY ./docker/php/php.ini /usr/local/etc/php/php.ini

WORKDIR /var/www/html
# RUN composer install -q -n --no-ansi --no-dev --no-scripts --no-progress --prefer-dist \
RUN composer install -q -n --no-ansi --no-scripts --no-progress --prefer-dist \
  && chown -R www-data:www-data ./ \
  && chmod -R 777 storage bootstrap/cache \
  && php artisan optimize:clear \
  && php artisan optimize \
  && php artisan cache:clear \
  && php artisan config:clear \
  && php artisan route:clear \
  && php artisan view:clear \
