# Ansible Automation Roles
This directory contains Ansible roles for installing Infrastructure components on OpenShift, including Red Hat OpenShift Platform Plus components.

## Role Structure
Roles will contain some combination of the following directories, as needed.

### tasks
Defines the tasks that are executed when the role is called. 

### templates
Contains jinja template files that are populated as needed by the role.

### files
Contains files that are used by the role, often containing data that will be used by the role that shouldn't be hardcoded into the role.

### defaults
Defines default variable values for executing the role.