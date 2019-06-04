FROM php:7.3.5-cli-alpine

ENV SHELL /bin/bash

RUN mkdir -p /vscode_dev

# node.js
RUN curl -sL https://deb.nodesource.com/setup_12.x | bash - \
  && apt-get -y install nodejs

# Install git, process tools
RUN apt-get update && apt-get -y install \
  git \
  libpq-dev \
  libzip-dev \
  procps \
  zlib1g-dev

RUN docker-php-ext-install \
  zip \
  pdo_mysql

# Clean up
RUN apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

# composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
  && php -r "if (hash_file('SHA384', 'composer-setup.php') === '48e3236262b34d30969dca3c37281b3b4bbe3221bda826ac6a9a62d6444cdb0dcd0615698a5cbe587c3f0fe57a54d8f5') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
  && php composer-setup.php \
  && php -r "unlink('composer-setup.php');" \
  && mv composer.phar /usr/local/bin/composer

ENV COMPOSER_ALLOW_SUPERUSER 1

ENV COMPOSER_HOME /composer

ENV PATH $PATH:/composer/vendor/bin

RUN composer global require "laravel/installer"
