FROM    centos:6.4

RUN     rpm -Uvh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
RUN     yum install -y npm git

RUN mkdir -p /opt/docroot
WORKDIR /opt/docroot
RUN git clone https://github.com/etsy/statsd

WORKDIR /opt/docroot/statsd
ADD conf/conf.js /opt/docroot/statsd/conf.js

EXPOSE  8125/udp
CMD ["node", "stats.js","conf.js"]