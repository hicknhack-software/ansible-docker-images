ARG DISTRO=bionic
ARG ANSIBLE_VERSION=2.5.4
ARG ANSIBLE_GIT_BRANCH=v2.5.4
ARG ANSIBLE_DIR=/opt/ansible
ARG ANSIBLE_GIT_REPO=git://github.com/ansible/ansible.git

FROM ubuntu:${DISTRO} AS ansible
ARG DISTRO
ARG ANSIBLE_VERSION
ARG ANSIBLE_DIR
ARG ANSIBLE_GIT_REPO
ARG ANSIBLE_GIT_BRANCH

ENV \
  APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=1 \
  DEBIAN_FRONTEND=noninteractive \
  LANG=C.UTF-8 \
  LC_ALL=C.UTF-8

# Installing Ansible dependencies and Git
RUN \
  apt-get update --quiet \
  && apt-get install --yes --quiet \
  git \
  vim \
  python-yaml \
  python-paramiko \
  python-jinja2 \
  python-setuptools \
  sshpass \
  && apt-get --yes autoremove \
  && apt-get clean autoclean \
  && rm -rf /var/lib/apt/lists/{apt,dpkg,cache,log} /tmp/* /var/tmp/*

RUN \
  mkdir -p ${ANSIBLE_DIR} \
  && mkdir /ansible \
  && git clone --recurse-submodules --branch ${ANSIBLE_GIT_BRANCH} --depth 1 ${ANSIBLE_GIT_REPO} ${ANSIBLE_DIR} \
  && bash ${ANSIBLE_DIR}/hacking/env-setup

ENV ANSIBLE_DIR=${ANSIBLE_DIR}
ENV PATH=/opt/ansible/bin:/opt/ansible/test/runner:${PATH}
ENV PYTHONPATH=/opt/ansible/lib:${PYTHONPATH}
ENV MANPATH=/opt/ansible/docs/man:${MANPATH}
WORKDIR /ansible

CMD [ "ansible-playbook", "--version" ]
