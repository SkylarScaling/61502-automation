# OpenShift Infrastructure Automation
This directory contains Ansible playbooks and roles for installing Infrastructure components on OpenShift, including Red Hat OpenShift Platform Plus components.

**Note:**

Most playbooks include a commented out `vars_prompt` block that can be uncommented to prompt for required variables. 

Alternatively, the variables defined in that block can be provided by AAP job variables, included in an inventory file passed to the playbook using the `-i` argument with the CLI, or passed directly as arguments with the CLI.

Example:
```
ansible-playbook install-infra.yaml -i inventory
```

## Create Services Cluster
`create-services-cluster.yaml` will perform the full infrastructure installation via Ansible automation. 

This includes:
* Provisioning an OpenShift Hosted Control Planes (HCP) cluster on AWS
* Performing Day 2 Configuration

### Prerequisites
**TODO** Put in pre-reqs here

### Running the Playbook
Example:
```
ansible-playbook create-services-cluster.yaml -i <inventory file>
```

## Create Shared Cluster
`create-shared-cluster.yaml` will perform the full infrastructure installation via Ansible automation.

This includes:
* Provisioning an OpenShift Hosted Control Planes (HCP) cluster on AWS
* Performing Day 2 Configuration

### Prerequisites
**TODO** Put in pre-reqs here

### Running the Playbook
Example:
```
ansible-playbook create-shared-cluster.yaml -i <inventory file>
```

