FROM centos:centos6

RUN rpm -ivh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm

RUN yum -y update &&  yum install -y \
    libxml2-devel \
    libcurl-devel \
    enchant-devel \
    libpng-devel \
    telnet \
    git

RUN yum groupinstall -y 'Development Tools'

RUN ln -sf /usr/share/zoneinfo/Europe/London /etc/localtime
