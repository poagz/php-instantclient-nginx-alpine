FROM alpine:3.12
LABEL Maintainer="Pouya Aghazadeh"\
      Description="PHP Oracle Instant Client App Container"
ENV LD_LIBRARY_PATH /opt/oracle/instantclient_19_9/
ENV ORACLE_HOME /opt/oracle/instantclient_19_9/

COPY config/resolv.conf /etc/resolv.conf

RUN apk --no-cache add php7 php7-dev php7-fpm php7-opcache php7-mysqli php7-json php7-openssl php7-curl \
    php7-zlib php7-xml php7-phar php7-intl php7-dom php7-xmlreader php7-ctype php7-session \
    php7-mbstring php7-gd php7-pecl-yaml php7-soap php7-pecl-ssh2 php7-zip php7-pdo_mysql \
    nginx supervisor curl git composer libaio g++ gcc make libaio-dev php7-mcrypt php7-pdo \
    php7-iconv php7-pear php7-common php7-tokenizer php7-fileinfo gcompat libnsl openrc file re2c && \
    rm -rf /var/cache/apk/*
    
RUN ln -s /usr/lib/libnsl.so.2 /usr/lib/libnsl.so.1
RUN ln -s /usr/lib/libc.so /usr/lib/libresolv.so.2

RUN mkdir -p /opt/oracle


COPY oracle/instantclient-basic-linux.x64-19.9.0.0.0dbru.zip /opt/oracle
COPY oracle/instantclient-sdk-linux.x64-19.9.0.0.0dbru.zip /opt/oracle
COPY oracle/oci8-2.2.0.tgz /opt/oracle



RUN cd /opt/oracle \
    && unzip instantclient-sdk-linux.x64-19.9.0.0.0dbru.zip \
    && unzip instantclient-basic-linux.x64-19.9.0.0.0dbru.zip \
    && unzip pdo_oci.zip \
    && tar xzf oci8-2.2.0.tgz \
    && rm -rf instantclient-basic-linux.x64-19.9.0.0.0dbru.zip instantclient-sdk-linux.x64-19.9.0.0.0dbru.zip pdo_oci.zip oci8-2.2.0.tgz \
    && cd oci8-2.2.0 \
    && phpize \
    && ./configure --with-oci8=shared,instantclient,/opt/oracle/instantclient_19_9 \
    && make \
    && make install \
    && echo "extension=oci8.so" >> /etc/php7/conf.d/oci8.ini \
    && cd ../pdo_oci \
    && phpize \
    && ./configure --with-pdo-oci=instantclient,/opt/oracle/instantclient_19_9 \
    && make \
    && make install \
    && echo "extension=pdo_oci8.so" >> /etc/php7/conf.d/pdo_oci8.ini


RUN rm /etc/nginx/conf.d/default.conf

COPY config/nginx.conf /etc/nginx/nginx.conf
COPY config/fpm-pool.conf /etc/php7/php-fpm.d/www.conf
COPY config/php.ini /etc/php7/conf.d/custom.ini
COPY config/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

RUN mkdir -p /var/www/app

RUN chown -R nginx.nginx /var/www/app && \
    chown -R nginx.nginx /run && \
    chown -R nginx.nginx /var/lib/nginx && \
    chown -R nginx.nginx /var/log/nginx 

WORKDIR /var/www/app
COPY --chown=nginx src/ /var/www/app/


CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]

HEALTHCHECK --timeout=10s CMD curl --silent --fail http://127.0.0.1/fpm-ping



