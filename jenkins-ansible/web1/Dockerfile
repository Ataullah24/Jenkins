#Author:PG - Demo/Training/Testing

FROM remote-host

ENV container docker
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;
VOLUME [ "/sys/fs/cgroup" ]
RUN yum -y install openssh-server openssh-clients
RUN echo root:pass | chpasswd
RUN ssh-keygen -f /etc/ssh/ssh_host_rsa_key -N '' -t rsa -y
RUN ssh-keygen -f /etc/ssh/ssh_host_ed25519_key -N '' -t ed25519 -y

RUN yum -y update --nogpgcheck; yum clean all

RUN yum -y install openssh-server

#Install nginx repo
RUN rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm

# Install latest version of nginx
RUN yum install -y nginx --nogpgcheck

RUN echo "daemon off;" >> /etc/nginx/nginx.conf

#Install required php 5.6 packages
RUN yum install -y php php-fpm --nogpgcheck

#Update PHP configs
ADD ./php.ini /etc/php.ini
ADD ./www.conf /etc/php-fpm.d/www.conf

#Update nginx config
ADD ./default.conf /etc/nginx/conf.d/default.conf

ADD ./index.php /usr/share/nginx/html/index.php

# Install supervisor to run jobs
RUN yum install -y epel-release --nogpgcheck
RUN yum install -y supervisor --nogpgcheck

ADD ./supervisord.conf /etc/supervisord.conf

EXPOSE 8083
EXPOSE 443
EXPOSE 22

RUN /usr/sbin/sshd-keygen > /dev/null 2>&1

#Run nginx engine
CMD ["/usr/bin/supervisord","-n","-c","/etc/supervisord.conf","/usr/sbin/sshd","-D"]
