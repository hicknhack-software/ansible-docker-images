# Docker image to run Ansible

This Docker image aims to ensure that everybody is using the same Ansible environment for deploying to production.

The image is based on a selected Ubuntu distro and contains minimal Ansible (Core) installation.

Maintaining multiple versions of Ansible can be a hassle. Therefore we started to use a common docker image for all Devops activity.
This makes it easy to check new versions and upgrade the Ansible roles and playbooks.

## Usage

The default entry point is bash.

Old Python2 based Ansible versions:
```bash
docker run -it --rm --mount src="$(pwd)",target=/ansible,type=bind hnhs/ansible:py2-bionic-2.5.4 ansible-playbook ...
```

Latest Python3 based Ansible versions:
```bash
docker run -it --rm --mount src="$(pwd)",target=/ansible,type=bind hnhs/ansible:focal-2.11.3
```

We recommend to mount a volume for the ansible galaxy roles and collections.

## Build and Hack

All build arguments are introduced at the top of the Dockerfiles.

```bash
docker build --build-arg ANSIBLE_VERSION=2.5.4 --build-arg ANSIBLE_GIT_BRANCH=v2.5.4 -t ansible:py2-2.5.4 python2
```

```bash
docker build --build-arg ANSIBLE_VERSION=2.5.4 --build-arg ANSIBLE_GIT_BRANCH=v2.5.4 -t ansible:2.5.4 python3
```

```bash
docker build --build-arg ANSIBLE_VERSION=2.11.3 --build-arg ANSIBLE_GIT_BRANCH=v2.11.3 -t ansible:2.11.3 python3
```
