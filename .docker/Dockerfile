FROM php:7.2.11-apache

COPY ./.docker/apache2.conf /etc/apache2/apache2.conf

# Install other needed extensions
# Full list here : https://github.com/docker-library/php/issues/75
RUN apt-get update && apt-get install -y \
    && apt-get install -y libfreetype6-dev zlib1g-dev libjpeg62-turbo-dev libpng-dev && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install mbstring \
    && apt-get install -y mysql-client \
    && docker-php-ext-install mysqli \
    && apt-get install -y curl libcurl3 libcurl3-dev \
    && docker-php-ext-install curl \
    && docker-php-ext-install json \
    && docker-php-ext-install zip \
    && a2enmod rewrite \
    && a2enmod headers \
    && apt-get update -y

RUN apt-get install -y zlib1g-dev \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-install zip

RUN apt-get update && apt-get install -y libbz2-dev
RUN docker-php-ext-install bz2

#replace php ini file
COPY ./.docker/php.ini /usr/local/etc/php/conf.d/php.ini
COPY . /var/www/html/

RUN cd /var/www/html/
RUN usermod -a -G www-data root \
    && chown -R www-data:www-data /var/www \
    && chmod 2775 /var/www \
    && find /var/www -type d -exec chmod 2775 {} \; \
    && find /var/www -type f -exec chmod 0664 {} \;

WORKDIR /var/www/html/
RUN rm -rf .docker/*

RUN cd /var/www/html/

EXPOSE 80