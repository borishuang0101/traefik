FROM alpine:3.8
WORKDIR /build
RUN apk --no-cache add curl
RUN VERSION="7.2" \
    && NEED_APK="php7>$VERSION php7-fpm>$VERSION" \
    && apk --update add $NEED_APK

ENV PHP_DIR /etc/php7

RUN mkdir /volume \
    && mkdir /volume/log \
    && mkdir /volume/data \
    && mkdir /volume/config \

    && mv $PHP_DIR/php.ini $PHP_DIR/php.ini.bak \
    && ln -s /volume/config/php.ini $PHP_DIR/php.ini \

    && mv $PHP_DIR/php-fpm.conf $PHP_DIR/php-fpm.conf.bak \
    && ln -s /volume/config/php-fpm.conf $PHP_DIR/php-fpm.conf

VOLUME ["/volume/config", "/volume/data", "/volume/log"]

COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]

RUN addgroup www-data \
    && adduser -D www-data -G www-data

EXPOSE 9000/tcp

CMD /usr/sbin/php-fpm7 --nodaemonize
