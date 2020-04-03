## Get OS image
FROM centos:7

RUN yum -y update; yum clean all
RUN yum -y install systemd; yum clean all; \
(cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;
VOLUME [ "/sys/fs/cgroup" ]


# Configure the os
RUN yum -y update; yum -y install httpd httpd-tools; yum clean all; 

## install apache as a web server
COPY ./httpd/httpd.conf /usr/local/apache2/httpd/httpd.conf
#RUN echo "LoadModule rewrite_module modules/mod_rewrite.so" >> /etc/httpd/conf.modules.d/00-base.conf

RUN systemctl enable httpd.service

## Prepare php installation
RUN yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
RUN yum install -y yum install http://rpms.remirepo.net/enterprise/remi-release-7.rpm

RUN yum install -y yum-utils

# Install PHP
RUN yum-config-manager --enable remi-php56

RUN yum install -y php php-mcrypt php-cli php-gd php-curl php-mysql php-ldap php-zip php-fileinfo

## Install Phalcon For PHP
RUN yum install -y php-phalcon

# install phalcon.ini in the image
COPY ./phalcon.ini /etc/php.d/phalcon.ini

# install free tds in the server
RUN yum install -y php-mssql

EXPOSE 80

CMD ["/usr/sbin/httpd","-D","FOREGROUND"]
