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

Execute the ansible playbook:

```
ansible-playbook /home/username/rosa-automation/automation/create-services-cluster.yaml -i /home/username/path-to-inventory-file --vault-id @prompt
```

The above command will perform the full infrastructure installation via Ansible automation.


**Note:** The above command will execute the playbook with the following option `--vault-id @prompt` which is used inject the github token credentials for the secret `osrosa-acm-git-secret` secret in `acm-install-policies` namespace.


This includes:
* Provisioning an OpenShift Hosted Control Planes (HCP) cluster on AWS
* Performing Day 2 Configuration

### Prerequisites

1- Authenticate to AWS Cloud from the cli and verify the aws region depending on where the cluster will be provisioned.

2- Authenticate using rosa cli command by getting your rosa token from https://console.redhat.com/openshift/token/rosa 
Once logged in via rosa token with `rosa login --token=xxxxxxxx` verify you are authentication by running: `rosa whoami` this command will print out details about the AWS account, make sure the AWS account id matches the account id in the inventory file that is located in your home directory.

3- Inventory file, this file will live in your user home directory on the bastion host where the rosa-automation is cloned. the inventory file will contain values for different variables, review the inventory file before running the playbook.    

A- Verify the AWS Account ID is correct

B- Verify the AWS Region where the cluster is going to be deployed in

C- Update the value of `aws_iam_role_name` variable for creating a new role name to manage ESO secrets, example of role name: 

`ManagedOpenShift-HCP-access-to-ClusterName-eso-service-role`


D- Update the value of `aws_s3_role_name` variable for creating a new role name to manage the observability service, example of role name: 

`Managed-OpenShift-HCP-access-to-ClusterName-s3-service-role`

Note: When provisioning a new cluster, the following file must be updated `acm-policies/install-mco/config/observability.yml` to include the value of `aws_s3_role_name`  

4- Executing the ansible playbook, to execute the playbook make sure to navigate to rosa-automation directory in your user home directory first then execute the playbook:

```
cd rosa-automation
```

Generate secret.yaml in your user home directory for `osrosa-acm-git-secret` secret in `acm-install-policies` namespace, this secret is used to authenticate ROSA HCP cluster to github rosa-automation repo to deploy the ACM policies.

Generate secret.yaml via ansible-vault

```
 ansible-vault create secret.yaml
``` 
add a password for the vault file, then paste the content `username` & `github token`

To view the content of the secret.yaml file use: 

``` 
ansible-vault view secret.yaml
``` 
enter the password.

### Running the Playbook
Example:
```
ansible-playbook create-services-cluster.yaml -i <inventory file>
```

## Create Shared Cluster


### Prerequisites


### Running the Playbook
Example:
```
ansible-playbook create-shared-cluster.yaml -i <inventory file>
```

