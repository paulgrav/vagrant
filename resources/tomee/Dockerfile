# Tomee
#
# VERSION               0.0.4

FROM      ssdm-docker:5000/paulgrav/base
MAINTAINER Paul Grave "paul.grave@bskyb.com"

## Install some dependencies
RUN yum -y install apr-devel apr apr-util openssl-devel

# Download and unpack Tomee
RUN curl -O 'http://devbuild.localnet:8080/jenkins/job/tomee/4/artifact/*zip*/archive.zip'
RUN unzip archive.zip

## Install the JDK and setup some env vars
WORKDIR /usr/local
RUN curl -Lk -O -H "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/7u65-b17/jdk-7u65-linux-x64.tar.gz
RUN tar zxvf jdk-7u65-linux-x64.tar.gz
RUN rm -f jdk-7u65-linux-x64.tar.gz
ENV JAVA_HOME /usr/local/jdk1.7.0_65
ENV PATH $PATH:$JAVA_HOME/bin

## Build the APR native libraries for performance
WORKDIR /archive/bin
RUN tar xzvf tomcat-native.tar.gz
WORKDIR /archive/bin/tomcat-native-1.1.31-src/jni/native
RUN sh configure --with-apr=/usr/bin  --with-java-home=$JAVA_HOME --with-ssl=yes
RUN make
RUN make install

ENV JAVA_OPTS -Djava.library.path=/usr/local/apr/lib -server -XX:+UseG1GC -XX:MaxGCPauseMillis=2000 -XX:+DisableExplicitGC -XX:MaxPermSize=1024m

## Prepare to run Tomee
WORKDIR /archive/bin
EXPOSE 8080
CMD ["sh","catalina.sh", "run"]
