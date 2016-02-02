# Dockerfile with systemd/sshd for CentOS 7
#

FROM stackio/centos7-systemd

MAINTAINER Alexandre Mclean <alexandre.mclean@gmail.com>

RUN echo "===> Installing SSH..."        && \
    yum -y install openssh-server epel-release && \
    yum -y install pwgen && \

    echo "===> Configuring ssh..."    && \
    rm -f /etc/ssh/ssh_host_ecdsa_key /etc/ssh/ssh_host_rsa_key && \
    ssh-keygen -q -N "" -t dsa -f /etc/ssh/ssh_host_ecdsa_key && \
    ssh-keygen -q -N "" -t rsa -f /etc/ssh/ssh_host_rsa_key && \
    sed -i "s/#UsePrivilegeSeparation.*/UsePrivilegeSeparation no/g" /etc/ssh/sshd_config && \

    echo "===> Install sudo and don't require TTY..." && \
    yum -y install sudo && \
    sed -i -e 's/^\(Defaults\s*requiretty\)/#--- \1/'  /etc/sudoers

# configure sshd
ADD set_root_pw.sh /set_root_pw.sh
ADD run.sh /run.sh
RUN chmod +x /*.sh
ENV AUTHORIZED_KEYS **None**

RUN systemctl enable sshd.service

EXPOSE 22

CMD ["/run.sh"]
