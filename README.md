[![Build Status](https://travis-ci.com/oasis-roles/vm_deploy.svg?branch=master)](https://travis-ci.com/oasis-roles/vm_deploy)

vm_deploy
===========

Basic description for vm_deploy

Requirements
------------

Ansible 2.4 or higher

Red Hat Enterprise Linux 7 or equivalent

Valid Red Hat Subscriptions

Role Variables
--------------

Currently the following variables are supported:

### General

* `vm_deploy_become` - Default: true. If this role needs administrator
  privileges, then use the Ansible become functionality (based off sudo).
* `vm_deploy_become_user` - Default: root. If the role uses the become
  functionality for privilege escalation, then this is the name of the target
  user to change to.

Dependencies
------------

None

Example Playbook
----------------

```yaml
- hosts: vm_deploy-servers
  roles:
    - role: oasis_roles.vm_deploy
```

License
-------

GPLv3

Author Information
------------------

Author Name <authoremail@domain.net>