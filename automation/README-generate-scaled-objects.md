# Generating ScaledObjects for Workload Scheduling

There are 4 playbooks available for generating ScaledObjects for use with the Custom Metrics Autoscaler (CMA):

## generate-scaled-object.yaml

This playbook generates a single ScaledObject Custom Resource (CR) for a specified Deployment and Namespace.

You can run the playbook with the following command (replacing your inventory file as necessary):

```
ansible-playbook generate-scaled-object.yaml -i ~/my-inventory
```

Your inventory should look like the following (replacing values as necessary):

```yaml
openshift:
  hosts:
    localhost:
      ansible_connection: local
  vars:    
    deployment_name: quarkus-example
    deployment_namespace: quarkus-pipeline
    max_replicas: 10
    min_replicas: 0
    desired_replicas: 3
    cron_start: 0 6 * * 1,2,3,4,5
    cron_end: 0 18 * * 1,2,3,4,5
    cron_timezone: US/Mountain
```

## generate-scaled-objects-for-namespace.yaml

This playbook generates one ScaledObject Custom Resource (CR) for every Deployment in a single Namespace.

You can run the playbook with the following command (replacing your inventory file as necessary):

```
ansible-playbook generate-scaled-objects-for-namespace.yaml -i ~/my-inventory
```

Your inventory should look like the following (replacing values as necessary):

```yaml
openshift:
  hosts:
    localhost:
      ansible_connection: local
  vars:
    # Playbook must authenticate with cluster to fetch Deployments from Namespace
    ocp_cluster_address: shared-cluster-name.xXxX.p3.openshiftapps.com
    cluster_admin_user: ReplaceClusterAdminUser
    cluster_admin_password: ReplaceClusterAdminPassword
    
    # Only the namespace is required, not the Deployment name
    deployment_namespace: quarkus-pipeline
    max_replicas: 10
    min_replicas: 0
    desired_replicas: 3
    cron_start: 0 6 * * 1,2,3,4,5
    cron_end: 0 18 * * 1,2,3,4,5
    cron_timezone: US/Mountain
```

## generate-scaled-objects-for-namespace-list.yaml

This playbook generates one ScaledObject Custom Resource (CR) for every Deployment fetched from a list of Namespaces.

You can run the playbook with the following command (replacing your inventory file as necessary):

```
ansible-playbook generate-scaled-objects-for-namespace-list.yaml -i ~/my-inventory
```

Your inventory should look like the following (replacing values as necessary):

```yaml
openshift:
  hosts:
    localhost:
      ansible_connection: local
  vars:
    # Playbook must authenticate with cluster to fetch Deployments from Namespaces
    ocp_cluster_address: shared-cluster-name.xXxX.p3.openshiftapps.com
    cluster_admin_user: ReplaceClusterAdminUser
    cluster_admin_password: ReplaceClusterAdminPassword

    namespaces:
      - namespace-1
      - namespace-2
      - namespace-3
    
    max_replicas: 10
    min_replicas: 0
    desired_replicas: 3
    cron_start: 0 6 * * 1,2,3,4,5
    cron_end: 0 18 * * 1,2,3,4,5
    cron_timezone: US/Mountain
```

## generate-scaled-objects-from-files.yaml

This playbook generates one ScaledObject Custom Resource (CR) for every Deployment described in files defined in a target directory.

You can run the playbook with the following command (replacing your inventory file as necessary):

```
ansible-playbook generate-scaled-objects-from-files.yaml -i ~/my-inventory
```

Your inventory should look like the following (replacing values as necessary):

```yaml
openshift:
  hosts:
    localhost:
      ansible_connection: local
  vars:
    # Directory where your schedule files are defined
    source_dir: ~/test
```

Each file in the target directory (`source_dir`) should be formatted as follows (replacing values as necessary):

```yaml
deployment_name: quarkus-example
deployment_namespace: quarkus-pipeline
max_replicas: 10
min_replicas: 0
desired_replicas: 5
cron_start: 0 6 * * 1,2,3,4,5
cron_end: 0 18 * * 1,2,3,4,5
cron_timezone: US/Mountain
```

## References
[Custom Metrics Autoscaler (CMA) - OpenShift Docs](https://docs.openshift.com/container-platform/4.17/nodes/cma/nodes-cma-autoscaling-custom.html)

[Cron ScaledObject - KEDA Docs (CMA Upstream)](https://keda.sh/docs/2.16/scalers/cron/)

[Cron format for schedules](https://cloud.google.com/scheduler/docs/configuring/cron-job-schedules)