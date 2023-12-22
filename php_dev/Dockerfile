FROM docker.io/library/ubuntu:22.04

RUN apt update && DEBIAN_FRONTEND=noninteractive TZ=America/Toronto apt-get -y install tzdata \
&& apt install php8.1-fpm apache2 --no-install-recommends -y \
&& apt install gawk grep -y \
&& PHP_V=$(php --version | awk 'NR==1 {print $2}' | grep -oP '[0-9][0-9]*\.[0-9][0-9]*' -m 1 | head -n 1) \
&& echo "ProxyPassMatch ^/(.*\.php(/.*)?)$ fcgi://127.0.0.1:9000/var/www/html/\$1" >> /etc/apache2/sites-available/000-default.conf \
&& echo "DirectoryIndex /index.php index.php index.html" >> /etc/apache2/sites-available/000-default.conf \
&& sed -i 's#listen[[:blank:]]*=[[:blank:]]*/run/php/.*$#listen=9000#g' /etc/php/$PHP_V/fpm/pool.d/www.conf \
&& a2dismod mpm_prefork \
&& a2enmod mpm_event \
&& a2enmod proxy \
&& a2enmod proxy_fcgi \
&& service apache2 stop \
&& service php8.1-fpm start \
&& echo $PHP_V

CMD service php8.1-fpm start;apache2ctl -DFOREGROUND
EXPOSE 80
