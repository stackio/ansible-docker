# Dockerfile for building Ansible image for CentOS 6, with as few additional software as possible.
#
# [NOTE] To fix the "sudo: sorry, you must have a tty to run sudo" issue,
#        we need to patch /etc/sudoers.
#        @see http://unix.stackexchange.com/questions/122616/why-do-i-need-a-tty-to-run-sudo-if-i-can-sudo-without-a-password
#        @see https://bugzilla.redhat.com/show_bug.cgi?id=1020147
#
# Version  1.0
#
# Based on https://github.com/William-Yeh/docker-ansible/
#

# pull base image
FROM centos:centos6

MAINTAINER Alexandre Mclean <alexandre.mclean@gmail.com>

ENV ANSIBLE_VERSION 1.9.2

RUN echo "===> Installing EPEL..."        && \
    yum -y install epel-release           && \
    \
    \
    echo "==> Updating packages..." && \
    yum -y update && \
    \
    \
    echo "===> Installing dependencies..." && \
    yum -y install sudo python-pip python-pycrypto python-devel gcc git && \
    pip install -U pip                   && \
    \
    \
    echo "===> Installing ansible..."    && \
    pip install ansible==${ANSIBLE_VERSION}  && \
    \
    \
    echo "===> Disabling sudo 'requiretty' setting..."    && \
    sed -i -e 's/^\(Defaults\s*requiretty\)/#--- \1/'  /etc/sudoers   && \
    echo 'Defaults        env_keep += "http_proxy https_proxy HTTP_PROXY HTTPS_PROXY"' >> /etc/sudoers && \
    \
    \
    echo "===> Removing YUM build resources..."   && \
    yum -y remove epel-release gcc python-devel python-pycrypto python-pip && \
    yum clean all                                  && \
    \
    \
    echo "===> Adding hosts for convenience..."    && \
    mkdir -p /etc/ansible                          && \
    echo '[local]\nlocalhost\n' > /etc/ansible/hosts


# default command: display Ansible version
CMD [ "ansible-playbook", "--version" ]
