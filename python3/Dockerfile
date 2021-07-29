ARG DISTRO=focal
ARG ANSIBLE_VERSION=2.11.3
ARG ANSIBLE_GIT_BRANCH=v2.11.3
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
  && apt-get install --yes --quiet --no-install-recommends \
    git \
    vim \
    python3-yaml \
    python3-paramiko \
    python3-jinja2 \
    python3-setuptools \
    python3-packaging \
    python3-pip \
    sshpass \
  && ln -s /usr/bin/python3 /usr/bin/python \
  && pip install resolvelib==0.5.4 \
  && apt-get purge --yes --quiet \
    python3-pip \
  && apt-get --yes autoremove \
  && apt-get clean autoclean \
  && rm -rf /var/lib/apt/lists/{apt,dpkg,cache,log} /tmp/* /var/tmp/*

RUN \
  mkdir -p ${ANSIBLE_DIR} \
  && git clone --recurse-submodules --branch ${ANSIBLE_GIT_BRANCH} --depth 1 ${ANSIBLE_GIT_REPO} ${ANSIBLE_DIR} \
  && bash ${ANSIBLE_DIR}/hacking/env-setup

ENV ANSIBLE_DIR=${ANSIBLE_DIR}
ENV PATH=${ANSIBLE_DIR}/bin:${ANSIBLE_DIR}/test/runner:${PATH}
ENV PYTHONPATH=${ANSIBLE_DIR}/lib:${PYTHONPATH}
ENV MANPATH=${ANSIBLE_DIR}/docs/man:${MANPATH}
WORKDIR /ansible

# CMD [ "ansible-playbook", "--version" ]
