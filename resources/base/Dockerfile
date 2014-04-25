FROM centos

RUN rpm -ivh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm

RUN yum -y update
RUN yum groupinstall -y 'Development Tools'
RUN yum install -y libxml2-devel
RUN yum install -y libcurl-devel
RUN yum install -y enchant-devel
RUN yum install -y libpng-devel

RUN yum install -y git
