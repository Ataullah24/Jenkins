FROM centos:7

RUN yum -y install openssh-server

RUN useradd remote_user && \
    echo remote_user:"1234" | chpasswd && \
    mkdir /home/remote_user/.ssh && \
    chmod 700 /home/remote_user/.ssh

COPY remote-key.pub /home/remote_user/.ssh/authorized_keys

RUN chown remote_user:remote_user -R /home/remote_user/.ssh/ && \
    chmod 600 /home/remote_user/.ssh/authorized_keys

RUN ps -ef|grep sshd

RUN /usr/sbin/sshd-keygen

RUN yum -y install mysql

RUN yum -y install epel-release && \
    yum -y install python-pip && \
    pip install --upgrade pip && \
    pip install awscli

CMD /usr/sbin/sshd -D

