# Dockerfile for building Ansible image for Ubuntu 12.04 (precise), with as few additional software as possible.
#
# Version  1.0
#
# Based on https://github.com/William-Yeh/docker-ansible/
#


# pull base image
FROM ubuntu:12.04

MAINTAINER Alexandre Mclean <alexandre.mclean@gmail.com>

ENV ANSIBLE_VERSION 1.9.2

RUN echo "===> Adding Ansible's PPA..."  && \
    echo "deb http://ppa.launchpad.net/ansible/ansible/ubuntu precise main" | tee /etc/apt/sources.list.d/ansible.list           && \
    echo "deb-src http://ppa.launchpad.net/ansible/ansible/ubuntu precise main" | tee -a /etc/apt/sources.list.d/ansible.list    && \
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 7BB9C367    && \
    apt-get update -qqy  && \
    \
    \
    echo "===> Installing Ansible..."  && \
    apt-get install -y ansible=${ANSIBLE_VERSION}*  && \
    apt-get install -y git && \
    \
    \
    echo "===> Removing Ansible PPA..."  && \
    rm -rf /var/lib/apt/lists/*  /etc/apt/sources.list.d/ansible.list  && \
    \
    \
    echo "===> Adding hosts for convenience..."  && \
    echo '[local]\nlocalhost\n' > /etc/ansible/hosts


# default command: display Ansible version
CMD [ "ansible-playbook", "--version" ]
