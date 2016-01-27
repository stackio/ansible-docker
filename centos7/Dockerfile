# Dockerfile for building Ansible image + sshd for CentOS 7
# Based on tutum/centos
#

FROM centos:centos7

MAINTAINER Alexandre Mclean <alexandre.mclean@gmail.com>

ENV ANSIBLE_VERSION 1.9.4

RUN echo "===> Installing SSH..."        && \
    yum -y install openssh-server epel-release  pwgen && \

    echo "===> Installing ansible dependencies..." && \
    yum -y install sudo python-pip python-pycrypto python-devel gcc git && \
    pip install -U pip                   && \

    echo "===> Installing ansible..."    && \
    pip install ansible==${ANSIBLE_VERSION}  && \

    echo "===> Disabling sudo 'requiretty' setting..."    && \
    sed -i -e 's/^\(Defaults\s*requiretty\)/#--- \1/'  /etc/sudoers   && \
    echo 'Defaults        env_keep += "http_proxy https_proxy HTTP_PROXY HTTPS_PROXY"' >> /etc/sudoers && \

    echo "===> Configuring ssh..."    && \
    rm -f /etc/ssh/ssh_host_ecdsa_key /etc/ssh/ssh_host_rsa_key && \
    ssh-keygen -q -N "" -t dsa -f /etc/ssh/ssh_host_ecdsa_key && \
    ssh-keygen -q -N "" -t rsa -f /etc/ssh/ssh_host_rsa_key && \
    sed -i "s/#UsePrivilegeSeparation.*/UsePrivilegeSeparation no/g" /etc/ssh/sshd_config && \

    echo "===> Removing YUM build resources..."   && \
    yum -y remove epel-release gcc python-devel python-pycrypto python-pip && \
    yum clean all                                  && \

    echo "===> Adding hosts for convenience..."    && \
    mkdir -p /etc/ansible                          && \
    echo '[local]\nlocalhost\n' > /etc/ansible/hosts

ADD set_root_pw.sh /set_root_pw.sh
ADD run.sh /run.sh
RUN chmod +x /*.sh

ENV AUTHORIZED_KEYS **None**

EXPOSE 22
CMD ["/run.sh"]
