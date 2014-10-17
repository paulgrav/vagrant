# Glassfish4 + Oracle JDK 1.7.0_40 
#
# VERSION               0.0.4

FROM      paulgrav/base
MAINTAINER Paul Grave "paul.grave@bskyb.com"

WORKDIR /usr/local

RUN curl -Lk -O -H "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/7u65-b17/jdk-7u65-linux-x64.tar.gz
RUN curl -Lk -O -H "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.jboss.org/jbossas/7.1/jboss-as-7.1.1.Final/jboss-as-7.1.1.Final.zip"

RUN tar zxvf jdk-7u65-linux-x64.tar.gz
RUN rm -f jdk-7u65-linux-x64.tar.gz
RUN unzip jboss-as-7.1.1.Final.zip
RUN rm -f jboss-as-7.1.1.Final.zip

ENV JAVA_HOME /usr/local/jdk1.7.0_65

ENV JB_HOME /usr/local/jboss-as-7.1.1.Final

ENV PATH $PATH:$JAVA_HOME/bin:$JB_HOME/bin

# PORT FORWARD THE ADMIN PORT, HTTP LISTENER-1 PORT, HTTPS LISTENER PORT, PURE JMX CLIENTS PORT, MESSAGE QUEUE PORT, IIOP PORT, IIOP/SSL PORT, IIOP/SSL PORT WITH MUTUAL AUTHENTICATION
EXPOSE 9999 8080 4447 9990

CMD ["standalone.sh"]

