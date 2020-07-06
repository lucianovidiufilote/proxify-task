FROM php:7.4.7-apache-buster

RUN curl -sS https://getcomposer.org/installer | php
RUN chmod +x composer.phar
RUN mv composer.phar /usr/local/bin/composer
RUN composer -V

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y git

WORKDIR /var/www/html/

RUN git clone https://github.com/lucianovidiufilote/proxify.git
WORKDIR /var/www/html/proxify/
RUN composer install

WORKDIR /etc/apache2/sites-available/
COPY docker/webservice/proxify.local.conf proxify.local.conf
RUN a2ensite proxify.local
RUN a2enmod rewrite

WORKDIR /etc/apache2/
RUN echo "ServerName localhost" >> apache2.conf

#supervisor setup
RUN apt-get install supervisor -y
WORKDIR /etc/supervisor/
COPY docker/webservice/supervisord.conf supervisord.conf
WORKDIR /etc/supervisor/conf.d
COPY docker/webservice/messenger-worker.conf messenger-worker.conf

RUN docker-php-ext-install pdo_mysql
EXPOSE 80

WORKDIR /var/www/html/proxify/
CMD ["php bin/console doctrine:database:create"]
CMD ["php bin/console doctrine:schema:update --force"]

#Alternative that works both for Docker Linux and Windows
CMD "127.0.0.1 proxify.local" >> /etc/hosts;
CMD ["/usr/bin/supervisord"]
CMD ["apache2-foreground"]