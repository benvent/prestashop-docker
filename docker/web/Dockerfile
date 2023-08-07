FROM php:8.1-apache

# Check on local system with command 'id -u' and 'id -d'
ARG USER_ID=1000
ARG GROUP_ID=1000

COPY vhost.conf /etc/apache2/sites-available/000-default.conf
COPY server.crt /etc/apache2/ssl/server.crt
COPY server.key /etc/apache2/ssl/server.key

# Update apt
RUN apt-get update -qq
RUN apt-get upgrade -qqy

# Install zip extension
RUN apt-get install -y libzip-dev zip
RUN docker-php-ext-install zip

# Get packages
RUN apt-get install -y wget gnupg curl openssh-client rsync grsync git libicu-dev libxslt-dev
#RUN apt-get install -y --no-install-recommends git
RUN docker-php-ext-install pdo pdo_mysql mysqli intl xsl
RUN docker-php-ext-install bcmath
RUN docker-php-ext-install ctype

# Install GD
RUN apt-get install -y libwebp-dev libjpeg-dev libpng-dev libxpm-dev libfreetype6-dev
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install gd

# Get Node.js
RUN curl -sL https://deb.nodesource.com/setup_18.x | bash -
RUN apt install -y --no-install-recommends nodejs
RUN npm install -g npm@latest

# Get Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php --filename=composer --install-dir=/usr/local/bin
RUN php -r "unlink('composer-setup.php');"

# Active mod_rewrite & mod_ssl
RUN a2enmod rewrite
RUN a2enmod ssl

ARG USER_ID=1000
ARG GROUP_ID=1000
# To run files with the same group as your primary user
RUN groupmod -g $GROUP_ID www-data \
  && usermod -u $USER_ID -g $GROUP_ID www-data
