FROM nginx:alpine3.18
LABEL maintainer="vaughngx4 (vaughng@pm.me)"
WORKDIR /
RUN mkdir -p /usr/socioboard/app
RUN apk update && apk add --no-cache \
    bash \
    jq \
    composer
RUN apk update && apk add --no-cache \
    php81 \
    php81-fpm \
    php81-session \
    php81-dom \
    php81-tokenizer \
    php81-fileinfo \
    php81-apache2 \
    php81-mysqli \
    php81-opcache \
    php81-common \
    php81-cli \
    php81-curl \
    php81-xml \
    php81-xmlwriter
RUN rm -rf /var/cache/apk/*
WORKDIR /usr/socioboard/app
#COPY ./php.ini /etc/php8/php.ini
RUN rm -f /etc/nginx/conf.d/default.conf
COPY ./socioboard-web-php ./socioboard-web-php
RUN cd ./socioboard-web-php && composer update phpspec/prophecy phpunit/phpunit && composer install
RUN chown -R nginx:nginx /usr/socioboard/app && \
    sed -i "s;nobody;nginx;g" /etc/php81/php-fpm.d/www.conf
COPY ./nginx.conf /etc/nginx/nginx.conf
COPY ./nginx-socioboard-web.conf /etc/nginx/templates/socioboard.conf.template
COPY ./entrypoint.sh /docker-entrypoint.sh
COPY ./init.sh /init.sh
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD /usr/sbin/nginx && php-fpm81 -O && tail -f /dev/null
