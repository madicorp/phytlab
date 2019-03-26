FROM php:7.1-apache-stretch

ENV BOOKSTACK=BookStack \
    BOOKSTACK_VERSION=0.25.2 \
    BOOKSTACK_HOME="/var/www/phytlab"

ADD ./ $BOOKSTACK_HOME

RUN apt-get update && apt-get install -y git zlib1g-dev libfreetype6-dev libjpeg62-turbo-dev libmcrypt-dev libpng-dev wget libldap2-dev libtidy-dev \
   && docker-php-ext-install pdo pdo_mysql mbstring zip tidy \
   && docker-php-ext-configure ldap \
   && docker-php-ext-install ldap \
   && docker-php-ext-configure gd --with-freetype-dir=usr/include/ --with-jpeg-dir=/usr/include/ \
   && docker-php-ext-install gd

RUN cd /var/www && curl -sS https://getcomposer.org/installer | php \
   && mv /var/www/composer.phar /usr/local/bin/composer \
   && cd $BOOKSTACK_HOME && composer install \
   && chown -R www-data:www-data $BOOKSTACK_HOME \
   && apt-get -y autoremove \
   && apt-get clean \
   && rm -rf /var/lib/apt/lists/* /var/tmp/* /etc/apache2/sites-enabled/000-*.conf

COPY docker/phytlab/conf/php.ini /usr/local/etc/php/php.ini
COPY docker/phytlab/conf/phytlab.conf /etc/apache2/sites-enabled/phytlab.conf
RUN a2enmod rewrite

COPY docker/phytlab/docker-entrypoint.sh /
RUN chmod +x /docker-entrypoint.sh

WORKDIR $BOOKSTACK_HOME

EXPOSE 80

VOLUME ["$BOOKSTACK_HOME/public/uploads","$BOOKSTACK_HOME/public/storage"]

ENTRYPOINT ["/docker-entrypoint.sh"]

ARG BUILD_DATE
ARG VCS_REF
LABEL org.label-schema.build-date=$BUILD_DATE \
      org.label-schema.docker.dockerfile="/Dockerfile" \
      org.label-schema.license="License-GPLv3" \
      org.label-schema.name="phytlab" \
      org.label-schema.vendor="madicorp" \
      org.label-schema.url="https://github.com/madicorp/phytlab" \
      org.label-schema.vcs-ref=$VCS_REF \
      org.label-schema.vcs-url="https://github.com/madicorp/phytlab.git" \
      org.label-schema.vcs-type="Git"