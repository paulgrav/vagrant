FROM paulgrav/base

ENV PHP_VERSION 5.5.11
ENV PHP_IMAGICK_VERSION 3.1.2
ENV PHP_MCRYPT_FILTER_VERSION 0.1.0
ENV PHP_MONGO_VERSION 1.4.5
ENV PHP_OAUTH_VERSION 1.2.3
ENV PHP_REDIS_VERSION 2.2.4
ENV PHP_XCACHE_VERSION 3.1.0

RUN useradd httpd

ADD php-fpm /etc/init.d/php-fpm
ADD php.ini /opt/php/etc/php.ini
ADD php-fpm.conf /opt/php/etc/php-fpm.conf

RUN chown httpd:httpd /opt/php/etc/php-fpm.conf
RUN chown httpd:httpd /etc/init.d/php-fpm
RUN chown httpd:httpd /opt/php/etc/php.ini

WORKDIR /tmp
RUN curl http://uk1.php.net/distributions/php-$PHP_VERSION.tar.bz2 | tar xjvf -
RUN yum -y update
RUN yum groupinstall -y 'Development Tools'
RUN yum install -y libxml2-devel
RUN yum install -y libcurl-devel
RUN yum install -y enchant-devel
RUN yum install -y libpng-devel
# RUN yum install -y libXpm-devel
RUN yum install -y gmp-devel
RUN yum install -y libicu-devel
RUN yum install -y aspell-devel
RUN yum install -y openssl-devel
RUN yum install -y bzip2-devel
RUN yum install -y libjpeg-devel
RUN yum install -y freetype-devel
RUN yum install -y readline-devel
RUN yum install -y libtidy-devel
RUN yum install -y libxslt-devel
RUN yum install -y libmcrypt
RUN yum install -y libmcrypt-devel
# RUN yum install -y ImageMagick-devel

WORKDIR /tmp/php-5.5.11
RUN ./configure --prefix=/opt/php --enable-mbstring --enable-opcache --with-enchant --enable-pcntl --enable-mbregex --with-gd --enable-zip --enable-fpm --with-fpm-user=httpd --with-curl --with-fpm-group=httpd
RUN make && make install

WORKDIR /tmp
RUN curl http://pecl.php.net/get/mcrypt_filter-0.1.0.tgz | tar xzvf -
WORKDIR /tmp/mcrypt_filter-0.1.0
RUN /opt/php/bin/phpize
RUN ./configure --with-php-config=/opt/php/bin/php-config
RUN make install

WORKDIR /tmp
RUN curl http://pecl.php.net/get/mongo-1.4.5.tgz | tar xzvf -
WORKDIR /tmp/mongo-1.4.5
RUN /opt/php/bin/phpize
RUN ./configure --with-php-config=/opt/php/bin/php-config
RUN make install

WORKDIR /tmp
RUN curl http://pecl.php.net/get/oauth-1.2.3.tgz | tar xzvf -
WORKDIR /tmp/oauth-1.2.3
RUN /opt/php/bin/phpize
RUN ./configure --with-php-config=/opt/php/bin/php-config
RUN make install

WORKDIR /tmp
RUN git clone https://github.com/nicolasff/phpredis.git
WORKDIR /tmp/phpredis
RUN git checkout $PHP_REDIS_VERSION
RUN /opt/php/bin/phpize
RUN ./configure --with-php-config=/opt/php/bin/php-config
RUN make install

WORKDIR /tmp
RUN curl http://xcache.lighttpd.net/pub/Releases/3.1.0/xcache-3.1.0.tar.bz2 | tar xjvf -
WORKDIR /tmp/xcache-3.1.0
RUN /opt/php/bin/phpize
RUN ./configure --with-php-config=/opt/php/bin/php-config
RUN make install

WORKDIR /tmp
RUN curl http://pecl.php.net/get/xdebug-2.2.4.tgz | tar xzvf -
WORKDIR /tmp/xdebug-2.2.4
RUN /opt/php/bin/phpize
RUN ./configure --with-php-config=/opt/php/bin/php-config
RUN make install


RUN chown -R httpd:httpd /opt/php
EXPOSE 9000
CMD ["-y","/opt/php/etc/php-fpm.conf"]
ENTRYPOINT ["/opt/php/sbin/php-fpm","-c","/opt/php/etc/php.ini"]