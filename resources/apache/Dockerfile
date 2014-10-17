FROM paulgrav/base

RUN useradd httpd

WORKDIR /tmp
RUN curl http://mirror.vorboss.net/apache//httpd/httpd-2.2.27.tar.bz2 | tar xjvf -
WORKDIR /tmp/httpd-2.2.27
RUN ./configure --prefix=/opt/apache --with-mpm=worker --enable-cgi --enable-vhost-alias --enable-unixd --enable-rewrite --enable-http --enable-proxy --enable-headers --enable-expires --enable-so --enable-mods-shared="all"
RUN make
RUN make install

WORKDIR /tmp
RUN curl http://www.fastcgi.com/dist/mod_fastcgi-2.4.6.tar.gz | tar xzvf -
WORKDIR /tmp/mod_fastcgi-2.4.6
RUN cp Makefile.AP2 Makefile
RUN make top_dir=/opt/apache
RUN make install top_dir=/opt/apache

ADD conf/httpd.conf /opt/apache/conf/httpd.conf
ADD conf/vhosts/skysports.conf /opt/apache/conf/vhosts/skysports.conf
# ADD php-fpm.conf /opt/php/etc/php-fpm.conf
# VOLUME ["/opt/docroot"]

RUN chown -R httpd:httpd /opt/apache
EXPOSE 80
ENTRYPOINT ["/opt/apache/bin/httpd"]
CMD ["-D", "FOREGROUND"]