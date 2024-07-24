# OpenShift Infrastructure Automation
This directory contains Ansible playbooks and roles for installing Infrastructure components on OpenShift, including Red Hat OpenShift Platform Plus components.

**Note:**

Most playbooks include a commented out `vars_prompt` block that can be uncommented to prompt for required variables. 

Alternatively, the variables defined in that block can be provided by AAP job variables, included in an inventory file passed to the playbook using the `-i` argument with the CLI, or passed directly as arguments with the CLI.

Example:
```
ansible-playbook install-infra.yaml -i inventory
```

## Install Infra
`install-infra` will perform the full infrastructure installation via Ansible automation. 

This includes:
* Installing Advanced Cluster Management (ACM) to OCP hub cluster
* Installing Advanced Cluster Security (ACS) via ACM subscription to hub and managed clusters
* Installing Local Storage Operator (LSO) via subscription to hub cluster
* Installing Open Data Foundations (ODF) via subscription to hub cluster
* Installing ACM Multi-cluster Observability (MCO) via subscription to hub cluster
* Installing Quay and Container Storage Operator (CSO) via subscription to hub cluster, and Quay bridge operator to hub and managed clusters
* Creating ACS backup cronjob on ACS Central / ACM Hub cluster

Example:
```
ansible-playbook install-infra.yaml
```

## Additional Playbooks
The additional playbooks in this directory can perform the individual actions that are included in the `install-infra` playbook, and are primarily included for testing purposes.