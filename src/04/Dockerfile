# Descarga la imagen base: Ubuntu 24.04 (noble)
FROM ubuntu:24.04

# Instala nginx, PHP 8.3 (php-fpm) y supervisord desde los paquetes de la distribución
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        nginx php8.3-fpm supervisor && \
    rm -rf /var/lib/apt/lists/*

# Define las variables de entorno
ENV nginx_vhost=/etc/nginx/sites-available/default
ENV php_conf=/etc/php/8.3/fpm/php.ini
ENV nginx_conf=/etc/nginx/nginx.conf
ENV supervisor_conf=/etc/supervisor/supervisord.conf

# Habilita la configuración del servidor virtual de php-fpm en nginx
COPY default ${nginx_vhost}
RUN sed -i -e 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g' ${php_conf} && \
    echo "\ndaemon off;" >> ${nginx_conf}

# Copia la configuración del supervisor
COPY supervisord.conf ${supervisor_conf}

RUN mkdir -p /run/php && \
    chown -R www-data:www-data /var/www/html && \
    chown -R www-data:www-data /run/php

# Configura el volumen de almacenamiento
VOLUME ["/etc/nginx/sites-enabled", "/etc/nginx/certs", "/etc/nginx/conf.d", "/var/log/nginx", "/var/www/html"]

# Configura los servicios y los puertos
COPY start.sh /start.sh
CMD ["/start.sh"]

EXPOSE 80 443
