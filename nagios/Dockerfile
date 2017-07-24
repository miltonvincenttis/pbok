FROM centos:centos7.3.1611
MAINTAINER milton.vincenttis@gmail.com

#--- prerequisites


#--- INSTALLING SELINUX
RUN yum -y install policycoreutils policycoreutils-python selinux-policy selinux-policy-targeted libselinux-utils setroubleshoot-server setools setools-console mcstrans

#RUN senforce 0

#RUN sed -i 's/SELINUX=.*/SELINUX=disabled/g' /etc/selinux/config


RUN yum -y install gcc make imake binutils cpp \
	postgresql-devel mysql-libs mysql-devel \
	openssl openssl-devel pkgconfig \
	gd gd-devel gd-progs libpng libpng-devel \
	libjpeg libjpeg-devel perl perl-devel \
	net-snmp net-snmp-devel net-snmp-perl net-snmp-utils \
	httpd php unzip nano wget


RUN yum -y install wget

#--- DOWNLOAD, AND EXPLODE NAGIOS CORE SOURCE
RUN cd /tmp
RUN wget -c -O /tmp/nagios-src https://github.com/NagiosEnterprises/nagioscore/archive/nagios-4.3.2.tar.gz

RUN mkdir /usr/src/nagios4
RUN cd /usr/src/nagios4
RUN tar -xvf /tmp/nagios-src 

#--- DOWNLOAD, AND EXPLODE PLUGINS SOURCE 
RUN wget -c -O /tmp/nagios-plugins-src http://www.nagios-plugins.org/download/nagios-plugins-2.2.1.tar.gz

RUN cd /usr/src/nagios4
RUN tar -xvf /tmp/nagios-plugins-src  


RUN groupadd nagios
RUN groupadd nagioscmd
RUN useradd -g nagios -G nagioscmd -d /opt/nagios nagios


#--- RUN THIS TO FIND OUT APACHE2 USER: $ grep -r ^User /etc/apache* /etc/httpd*

RUN usermod -G nagioscmd apache

#--- COMPILING AND INSTALLING NAGIOS
RUN mkdir -p /opt/nagios /etc/nagios /var/nagios
RUN chown nagios:nagios /opt/nagios /etc/nagios /var/nagios

RUN cd /usr/src/nagios4/nagioscore-nagios-4.3.2

RUN	sh configure \
	--prefix=/opt/nagios \
	--sysconfdir=/etc/nagios \
	--localstatedir=/var/nagios \
	--libexecdir=/opt/nagios/plugins \
	--with-command-group=nagioscmd


RUN make all
RUN make install
RUN make install-init
RUN make install-commandmode
RUN make install-config
RUN make install-webconf
RUN make install-classicui


#--- COMPILE PLUGINS AND INSTALLING NAGIOS PLUGINS
 
RUN cd /usr/src/nagios4/nagios-plugins-2.2.1

RUN sh 	configure \
	--prefix=/opt/nagios \
	--sysconfdir=/etc/nagios \
	--localstatedir=/var/nagios \
	--libexecdir=/opt/nagios/plugins

RUN make all
RUN make install

RUN htpasswd -c -b admin /usr/local/nagios/etc/htpasswd.users nagiosadmin 

RUN echo '#!/bin/bash'             	> /usr/sbin/my_init
RUN echo /etc/init.d/nagios restart     >> /usr/sbin/my_init
RUN echo /usr/sbin/httpd -k start 	>> /usr/sbin/my_init
RUN chmod ugo+rwx  /usr/sbin/my_init

#RUN /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg

RUN /usr/sbin/my_init

CMD ["/sbin/init"]
