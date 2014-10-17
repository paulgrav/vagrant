# Glassfish4 + Oracle JDK 1.7.0_40 
#
# VERSION               0.0.4

FROM      ssdm-docker:5000/paulgrav/base
MAINTAINER Paul Grave "paul.grave@bskyb.com"

# USER glassfish

WORKDIR /usr/local

# Install JDK
RUN curl -Lk -O -H "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/7u65-b17/jdk-7u65-linux-x64.tar.gz
RUN tar zxvf jdk-7u65-linux-x64.tar.gz
RUN rm -f jdk-7u65-linux-x64.tar.gz

# Install Glassfish
RUN curl -Lk -O -H "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.java.net/glassfish/3.1.2.2/release/glassfish-3.1.2.2.zip"
RUN unzip glassfish-3.1.2.2.zip && rm glassfish-3.1.2.2.zip

# Set some envs
ENV JAVA_HOME /usr/local/jdk1.7.0_65
ENV GF_HOME /usr/local/glassfish3
ENV PATH $PATH:$JAVA_HOME/bin:$GF_HOME/bin

# Install Jersey
RUN curl -o $GF_HOME/glassfish/domains/domain1/lib/jersey-bundle-1.18.jar http://repo1.maven.org/maven2/com/sun/jersey/jersey-bundle/1.18/jersey-bundle-1.18.jar
RUN rm $GF_HOME/glassfish/modules/jersey*.jar
RUN curl -o $GF_HOME/glassfish/domains/domain1/lib/asm-3.1.jar http://repo1.maven.org/maven2/asm/asm/3.1/asm-3.1.jar


# Install OJDBC
COPY ojdbc6.jar $GF_HOME/glassfish/domains/domain1/lib/ojdbc6.jar

COPY domain.xml $GF_HOME/glassfish/domains/domain1/config/

# Final Glassfish Prep
RUN chmod +x $GF_HOME/bin/*
RUN echo "admin;{SSHA256}80e0NeB6XBWXsIPa7pT54D9JZ5DR5hGQV1kN1OAsgJePNXY6Pl0EIw==;asadmin" > $GF_HOME/glassfish/domains/domain1/config/admin-keyfile
RUN echo "AS_ADMIN_PASSWORD=glassfish" > pwdfile
RUN \
  $GF_HOME/bin/asadmin start-domain && \
  $GF_HOME/bin/asadmin --user admin --passwordfile pwdfile enable-secure-admin && \
  $GF_HOME/bin/asadmin stop-domain

# Download and install SHAKIRA
RUN mkdir -p $GF_HOME/glassfish/domains/domain1/autodeploy/ && \
    curl -Lk -o $GF_HOME/glassfish/domains/domain1/autodeploy/SHAKIRA.war http://devbuild.localnet:8080/jenkins/job/Shakira/3938/artifact/dev/build/web/war/SHAKIRA.war

VOLUME ["$GF_HOME/glassfish/domains/domain1/autodeploy/"]



# docker run -e _JAVA_OPTIONS='-Dssdm.jdbc.url=jdbc:oracle:thin:@ora-dev-a.localnet:1521:DEV1' -i -p 8080:8080 -t ssdm-docker:5000/paulgrav/glassfish

# PORT FORWARD THE ADMIN PORT, HTTP LISTENER-1 PORT, HTTPS LISTENER PORT, PURE JMX CLIENTS PORT, MESSAGE QUEUE PORT, IIOP PORT, IIOP/SSL PORT, IIOP/SSL PORT WITH MUTUAL AUTHENTICATION
EXPOSE 4848 8080
CMD ["asadmin", "start-domain", "-v"]