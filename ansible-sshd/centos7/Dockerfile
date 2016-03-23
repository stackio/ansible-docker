# Dockerfile for building Ansible image + sshd for CentOS 7
#

FROM stackio/centos7-sshd

MAINTAINER Alexandre Mclean <alexandre.mclean@gmail.com>

ENV ANSIBLE_VERSION 1.9.5

RUN echo "===> Installing epel..."        && \
    yum -y install epel-release  && \

    echo "===> Installing ansible dependencies..." && \
    yum -y install sudo python-pip python-pycrypto python-devel gcc git && \
    pip install -U pip                   && \

    echo "===> Installing ansible..."    && \
    pip install ansible==${ANSIBLE_VERSION}  && \

    echo "===> Disabling sudo 'requiretty' setting..."    && \
    sed -i -e 's/^\(Defaults\s*requiretty\)/#--- \1/'  /etc/sudoers   && \
    echo 'Defaults        env_keep += "http_proxy https_proxy HTTP_PROXY HTTPS_PROXY"' >> /etc/sudoers && \

    echo "===> Removing YUM build resources..."   && \
    yum -y remove epel-release gcc python-devel && \
    yum clean all

CMD ["/run.sh"]
