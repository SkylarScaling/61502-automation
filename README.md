# Introduction
This repository contains re-usable playbooks and ACM policies for use with OpenShift Container Platform, using Ansible automation.

# Structure
## acm-policies
Contains ACM policies structured using the ACM policyGenerator and kustomize to configure components on managed clusters.

## automation
Contains playbooks for executing infrastructure automations, including applying the above noted ACM policies via Subscription.

# Using this Repository
## Installation Process
To work with this repository locally you will need clone it using https or ssh authentication.
> NOTE: If you wish to make changes, fork this repo first. And feel free to submit enhancements back here with a Pull Request!

For https:
```
git clone https://github.com/SkylarScaling/61502-automation.git
```

For ssh:
```
git clone git@github.com:SkylarScaling/61502-automation.git
```

# Assertions and Assumptions

This automation assumes the following conditions have been met.

- The following software dependencies have been installed:

## Prerequisites
### Software dependencies

There are a handful of tools that you'll need to be able to effectively work with this repository. Please make sure you have the following tools installed.
Any commands provided are for linux systems.

- [Python 3+](https://www.python.org/downloads/) Installed
- [pip and setuptools](https://packaging.python.org/en/latest/tutorials/installing-packages/#ensure-pip-setuptools-and-wheel-are-up-to-date) Installed
- [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) Installed
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) Installed

> NOTE: Ansible will need python 3 in order to use the required modules and libraries, so it is recommended that you use
> the following command to install Ansible:
>
> ```
> $ pip3 install ansible
> ```
> You can then confirm which python version is being used by running:
> ```
> $ ansible --version | grep "python version"
> ```
> Example output:
> ```
> python version = 3.6.8 (default, Mar 18 2021, 08:58:41) [GCC 8.4.1 20200928 (Red Hat 8.4.1-1)]
> ```

The utilities in the list below are also required, but can be installed automatically by executing the Ansible playbook listed below, once Ansible is installed.

- [OpenShift CLI](https://docs.openshift.com/container-platform/4.7/cli_reference/openshift_cli/getting-started-cli.html) Installed
- [Python Modules](https://docs.python.org/3/installing/index.html):
    - [setuptools](https://pypi.org/project/setuptools/)
    - [kubernetes](https://pypi.org/project/kubernetes/)
    - [openshift](https://pypi.org/project/openshift/)
    - [boto3](https://pypi.org/project/boto3/)

They can be automatically installed by running the following command:

```
$ ansible-playbook install-ocp-prerequisites.yaml --ask-become-pass
```

> **Notes:**
>
> *Arguments*
>
> `--ask-become-pass` This will prompt for your root password, so required python libraries can be installed.

You can force the OpenShift binaries to update (if you already have oc on your system), by using the following command:
```
$ ansible-playbook install-ocp-prerequisites.yaml --ask-become-pass -e force_update=true
```

> **Notes:**
>
> *Arguments*
>
> `force_update` This will tell Ansible to delete the existing oc binaries, forcing new versions to be installed.
