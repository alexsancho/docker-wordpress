FROM centos:7
MAINTAINER Alex Sancho <asancho@keclab.com>

RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
RUN rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm

# Install updates centos
RUN yum -y update

RUN yum -y install gcc libxml2-devel pkgconfig openssl-devel bzip2-devel \
libpng-devel libpng-devel libjpeg-devel libXpm-devel freetype-devel gmp-devel \
libmcrypt-devel mariadb-devel aspell-devel recode-devel httpd-devel \
xpdf unzip wget

RUN yum -y install php70w mod_php php70w-bcmath php70w-cli php70w-common \
php70w-devel php70w-gd php70w-intl php70w-mbstring php70w-mcrypt \
php70w-mysql php70w-opcache php70w-pecl-imagick php70w-pecl-memcached \
php70w-pecl-redis php70w-soap php70w-xml php70w-xmlrpc mod_ssl httpd

# tools
RUN yum -y install epel-release iproute at curl crontabs git

RUN yum clean all

ENV \

    # PHP settings
    PHP_MEMORY_LIMIT="128M" \
    PHP_MAX_INPUT_VARS="1000" \
    PHP_ERROR_LOG="/proc/self/fd/1" \
    PHP_ERROR_LOG_LEVEL="warning" \
    PHP_ERROR_LOG_MAX_LEN="8192" \
    PHP_SESSION_REDIS_DB="0" \
    PHP_SESSION_HANDLER="files" \

    # You should count the *.php files in your project and set this number to be bigger
    # $ find . -type f -print | grep php | wc -l
    PHP_OPCACHE_MAX_FILES="8000" \

    # Amount of memory in MB to allocate for opcache
    PHP_OPCACHE_MAX_MEMORY="128" \

    # This can be overidden by you, it's just default for us
    TZ="Europe/Berlin"

RUN echo 'date.timezone=Europe/Berlin' > /etc/php.d/00-docker-php-date-timezone.ini
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin

VOLUME /var/www

# Expose apache.
EXPOSE 80
EXPOSE 443

COPY run-httpd.sh /run-httpd.sh
RUN chmod -v +x /run-httpd.sh

CMD ["/run-httpd.sh"]
