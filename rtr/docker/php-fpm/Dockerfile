FROM php:fpm

RUN apt-get update \
  && apt-get install -y zlib1g-dev libicu-dev g++ \
  && apt-get install -y git \
  && docker-php-ext-configure intl && docker-php-ext-install intl \
  && docker-php-ext-configure pcntl  && docker-php-ext-install pcntl \
  && docker-php-ext-install pdo_mysql \
  && pecl install xdebug && docker-php-ext-enable xdebug \
  && docker-php-ext-install bcmath \
  && pecl install zip && docker-php-ext-enable zip \
  && apt-get install -y libldap2-dev -y && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && docker-php-ext-install ldap \
  && apt-get install -y libxml2-dev php-soap  && docker-php-ext-install soap \
  && docker-php-ext-install sockets \
  && pecl install redis && docker-php-ext-enable redis

# memcached module
RUN apt-get install -y libmemcached-dev unzip
RUN curl -o /root/memcached.zip https://github.com/php-memcached-dev/php-memcached/archive/php7.zip -L
RUN cd /root && unzip memcached.zip && rm memcached.zip && \
  cd php-memcached-php7 && \
  phpize && ./configure --enable-sasl && make && make install && \
  cd /root && rm -rf /root/php-memcached-* && \
  echo "extension=memcached.so" > /usr/local/etc/php/conf.d/memcached.ini  && \
  echo "memcached.use_sasl = 1" >> /usr/local/etc/php/conf.d/memcached.ini

COPY php-fpm.conf /etc/php-fpm.conf
COPY php.ini /usr/local/etc/php/php.ini

RUN useradd -ms /bin/bash cristian
USER cristian
WORKDIR /app
