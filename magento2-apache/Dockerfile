FROM php:7.0.17-apache
MAINTAINER Loi Vo <it.tanloi@gmail.com>

# Change default .bashrc file
COPY root/.bashrc /root/.bashrc

# Add Kick-start script
COPY bin/kick-start /usr/local/bin/
COPY bin/composer /usr/local/bin/
RUN chmod -v +x /usr/local/bin/composer

# Copy php.ini.sample from Magento php.ini.sample
COPY config/php.ini.sample /usr/local/etc/php/php.ini

RUN apt-get update && apt-get install -y \
	vim \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
	libxslt-dev \
	libicu-dev \
&& docker-php-ext-install -j$(nproc) iconv mcrypt \
&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
&& docker-php-ext-install -j$(nproc) gd \
&& docker-php-ext-install -j$(nproc) xsl \
&& docker-php-ext-install -j$(nproc) intl \
&& docker-php-ext-install -j$(nproc) zip \
&& docker-php-ext-install -j$(nproc) bcmath \
&& docker-php-ext-install -j$(nproc) pdo_mysql 

RUN pecl install xdebug

# Enable ssl

RUN a2enmod rewrite
RUN a2enmod ssl
ADD config/default-ssl.conf /etc/apache2/sites-enabled/
ADD config/apache2.conf /etc/apache2/
ADD ssl /etc/ssl

EXPOSE 80 443

VOLUME ["/var/www/html", "/var/log/apache2","/etc/ssl/certs"]