FROM php:5.6-fpm

COPY ./sources.list.jessie /etc/apt/sources.list

#更新apt-get源 使用163的源
#RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak && \
#    echo "deb http://mirrors.163.com/debian/ jessie main non-free contrib" >/etc/apt/sources.list && \
#    echo "deb http://mirrors.163.com/debian/ jessie-proposed-updates main non-free contrib" >>/etc/apt/sources.list && \
#    echo "deb-src http://mirrors.163.com/debian/ jessie main non-free contrib" >>/etc/apt/sources.list && \
#    echo "deb-src http://mirrors.163.com/debian/ jessie-proposed-updates main non-free contrib" >>/etc/apt/sources.list


# Extensions: ctype, dom, fileinfo, ftp, hash, iconv, json, pdo, pdo_sqlite, session,
# tokenizer, simplexml, xml, xmlreader, xmlwriter and phar are bundled and compiled into
# PHP by default. If missing, install them directly by `docker-php-ext-install extension_name`

# Notice:
# 1. Mcrypt was DEPRECATED in PHP 7.1.0, and REMOVED in PHP 7.2.0.
# 2. opcache requires PHP version >= 7.0.0.
# 3. soap requires libxml2-dev.
# 4. xml, xmlrpc, wddx require libxml2-dev and libxslt-dev.
# 5. Line `&& :\` is just for better reading and do nothing.

RUN apt-get update \
&& docker-php-ext-install mysqli \
&& docker-php-ext-install mbstring \
&& docker-php-ext-install bcmath \
&& apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev \
&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
&& docker-php-ext-install gd \
&& docker-php-ext-install gettext \
&& apt-get install -y libmcrypt-dev \
&& docker-php-ext-install mcrypt \
&& apt-get install -y libmemcached-dev zlib1g-dev \
&& apt-get install -y libssl-dev \
&& docker-php-ext-install mysql \
&& docker-php-ext-install pcntl \
&& docker-php-ext-install pdo_mysql \
&& apt-get install -y libpq-dev \
&& docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
&& docker-php-ext-install pdo pdo_pgsql pgsql \
&& docker-php-ext-install shmop \
&& apt-get install -y libxml2-dev \
&& docker-php-ext-install soap \
&& docker-php-ext-install sockets \
&& docker-php-ext-install sysvsem \
&& apt-get install -y libxslt-dev \
&& docker-php-ext-install xmlrpc \
&& docker-php-ext-install zip


# Composer
RUN php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /bin/composer \
    && composer config -g repo.packagist composer https://packagist.phpcomposer.com

# Install extension using pecl
# Notice: if pecl install get error
#    `No releases available for package "pecl.php.net/xxx"`
# or
#    `Package "xxx" does not have REST xml available`
# Please turn on proxy (The proxy IP may be docker host IP or others):
#RUN pear config-set http_proxy http://10.0.75.1:1080

RUN pecl install xdebug-2.5.5 \
    && docker-php-ext-enable xdebug \
    && :\
    && pecl install redis-3.1.4 \
    && docker-php-ext-enable redis \
    && :\
    && pecl install memcache \
    && docker-php-ext-enable memcache \
    && :\
    && pecl install memcached-2.2.0 \
    && docker-php-ext-enable memcached \
    && :\
    && pecl install mongo \
    && docker-php-ext-enable mongo