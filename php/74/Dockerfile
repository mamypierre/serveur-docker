FROM php:7.4-fpm

ARG WEB_USER
ARG WEB_GROUP
ARG PHP_ROOT_DIR
ARG WORK_ROOT_DIR

COPY www.conf ${PHP_ROOT_DIR}/php-fpm.d/www.conf
COPY php.ini ${PHP_ROOT_DIR}/php/conf.d/php.ini
COPY xdebug.ini ${PHP_ROOT_DIR}/php/conf.d/xdebug.ini
COPY error_reporting.ini ${PHP_ROOT_DIR}/php/conf.d/error_reporting.ini

#RUN addgroup -g 1000 ${WEB_GROUP} && adduser -G ${WEB_USER} -g ${WEB_USER} -s /bin/sh -D ${WEB_USER}
RUN usermod -u 1000 ${WEB_USER}
RUN groupmod -g 1000 ${WEB_GROUP}

RUN mkdir -p ${WORK_ROOT_DIR}
RUN chown ${WEB_USER}:${WEB_GROUP} ${WORK_ROOT_DIR}
WORKDIR ${WORK_ROOT_DIR}
RUN chgrp -R staff ${PHP_ROOT_DIR}/php-fpm.d/www.conf
RUN chown 777 -R ${WORK_ROOT_DIR}
RUN chmod u=rwx,g=rwx,o=rwx -R ${WORK_ROOT_DIR}
#module
RUN apt-get update -y
RUN apt-get install -y apt-utils
RUN apt-get install libtool -y
RUN apt-get install libcurl4-openssl-dev -y
RUN apt-get install libxml2-dev  -y
RUN apt-get install libpng-dev  -y
RUN apt-get install libjpeg-dev  -y
RUN apt-get install libfreetype6-dev  -y
RUN apt-get install libxslt-dev  -y
RUN apt-get install libzip-dev -y
RUN apt-get install libonig-dev -y
RUN apt-get install nginx -y


#extension
RUN docker-php-ext-install bcmath
RUN docker-php-ext-install ctype
RUN docker-php-ext-install curl
RUN docker-php-ext-install dom
RUN docker-php-ext-install gd
RUN docker-php-ext-configure gd --with-jpeg --with-freetype
RUN docker-php-ext-install -j$(nproc) gd
RUN docker-php-ext-install intl
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install mysqli
RUN docker-php-ext-install soap
RUN docker-php-ext-configure soap
RUN docker-php-ext-install xsl
RUN docker-php-ext-install zip
RUN docker-php-ext-configure zip
RUN docker-php-ext-install sockets
RUN docker-php-ext-install -j$(nproc) opcache
RUN docker-php-ext-install calendar && docker-php-ext-configure calendar
RUN docker-php-ext-install pcntl exif
RUN apt-get install -y git
#RUN docker-php-ext-install mcrypt
COPY docker-php-ext-opcache.ini ${PHP_ROOT_DIR}/php/conf.d/docker-php-ext-opcache.ini

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug
#composer



RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

#CMD ["service", "nginx", "start ;"]
#CMD ping localhost service nginx start
#ENTRYPOINT  /bin/bash
#exemple ENTRYPOINT service nginx start && service ssh start && /bin/bash "use && to separate your code"
