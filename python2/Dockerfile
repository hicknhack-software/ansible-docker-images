ARG DISTRO=bionic
ARG ANSIBLE_VERSION=2.5.4
ARG ANSIBLE_GIT_BRANCH=v2.5.4
ARG ANSIBLE_DIR=/opt/ansible
ARG ANSIBLE_GIT_REPO=https://github.com/ansible/ansible.git

FROM ubuntu:${DISTRO} AS ansible
ARG \
  DISTRO \
  ANSIBLE_VERSION \
  ANSIBLE_DIR \
  ANSIBLE_GIT_REPO \
  ANSIBLE_GIT_BRANCH \
  APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=1 \
  DEBIAN_FRONTEND=noninteractive
ENV \
  LANG=C.UTF-8 \
  LC_ALL=C.UTF-8

# Installing Ansible dependencies and Git
RUN <<INSTALL_PYTHON
  apt-get -qq update -o=Dpkg::Use-Pty=0
  apt-get -qq --yes install -o=Dpkg::Use-Pty=0 --no-install-recommends \
    ca-certificates \
    git \
    vim \
    python-yaml \
    python-paramiko \
    python-jinja2 \
    python-setuptools \
    sshpass
  apt-get -qq --yes autoremove -o=Dpkg::Use-Pty=0
  apt-get -qq clean autoclean -o=Dpkg::Use-Pty=0
  rm -rf /var/lib/apt/lists/{apt,dpkg,cache,log} /tmp/* /var/tmp/*
INSTALL_PYTHON

RUN <<INSTALL_ANSIBLE
  mkdir -p ${ANSIBLE_DIR}
  git clone --recurse-submodules --branch ${ANSIBLE_GIT_BRANCH} --depth 1 ${ANSIBLE_GIT_REPO} ${ANSIBLE_DIR}
  bash ${ANSIBLE_DIR}/hacking/env-setup
INSTALL_ANSIBLE

ENV \
  ANSIBLE_DIR=${ANSIBLE_DIR} \
  PATH=/opt/ansible/bin:/opt/ansible/test/runner:${PATH} \
  PYTHONPATH=/opt/ansible/lib:${PYTHONPATH} \
  MANPATH=/opt/ansible/docs/man:${MANPATH}
WORKDIR /ansible

CMD [ "ansible-playbook", "--version" ]
