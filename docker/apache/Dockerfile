FROM php:5-apache

# PHP extensions
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        jpegoptim \
    && pecl install xdebug-2.5.5 \
    && docker-php-ext-install -j$(nproc) mbstring pdo_mysql zip mcrypt tokenizer iconv mysqli \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-enable xdebug

RUN curl -o ioncube.tar.gz http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz \
    && tar -xvvzf ioncube.tar.gz \
    && mv ioncube/ioncube_loader_lin_5.6.so `php-config --extension-dir` \
    && rm -Rf ioncube.tar.gz ioncube \
    && docker-php-ext-enable ioncube_loader_lin_5.6

RUN curl -o pngout.tar.gz http://static.jonof.id.au/dl/kenutils/pngout-20150319-linux.tar.gz \
    && tar -xf pngout.tar.gz \
    && rm pngout.tar.gz \
    && cp pngout-20150319-linux/x86_64/pngout /usr/bin/pngout \
    && rm -rf pngout-20150319-linux

RUN pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini

# Apache modules
RUN a2enmod rewrite