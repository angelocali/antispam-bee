FROM php:8.0-alpine

WORKDIR /var/www/html

RUN apk update && apk add bash subversion mariadb-client
RUN docker-php-ext-install mysqli

# Install Composer
RUN EXPECTED_CHECKSUM=$(curl -s https://composer.github.io/installer.sig) \
    && curl https://getcomposer.org/installer -o composer-setup.php \
    && ACTUAL_CHECKSUM="$(php -r "echo hash_file('sha384', 'composer-setup.php');")" \
    && if [ "$EXPECTED_CHECKSUM" != "$ACTUAL_CHECKSUM" ]; then >&2 echo 'ERROR: Invalid installer checksum'; rm composer-setup.php; exit 1; fi \
    && php composer-setup.php --quiet \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /usr/local/bin/composer