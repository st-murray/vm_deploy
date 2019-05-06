[![Build Status](https://travis-ci.com/oasis-roles/vm_deploy.svg?branch=master)](https://travis-ci.com/oasis-roles/vm_deploy)

vm_deploy
===========

The role will:
Install the following packages on a hypervisor;
  - qemu-kvm
  - libvirt
  - virt-install
  - libguestfs-tools
It will then use libvirt and the ansible module virt-install to create
virtual images on the hypervisor with the option to specify multiple vm's
based on small, medium, and large vm specifications.
The VM's will use a kickstart file that is stored under templates.
The default root password for your VM's will be set to "redhat" but can be modified
by changing "Root password" setting in the anaconda-ks-cfg file.

This role is not idempotent due to the fact that virtual machines
will be shutdown and restarted during their creation.

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

* `vm_deploy_hypervisor:` - Required: The short name of the hypervisor

* `vm_deploy_domain_name:` - Required: The name to prefix your vms's with

* `vm_deploy_ssh_key:` - Required: The SSH key of the ansible controller
To enable password-less ssh follow something similar to
ssh root@yourhypervisor.com mkdir -p .ssh
cat ~/.ssh/id_rsa.pub | ssh root@yourhypervisor.com 'cat >> .ssh/authorized_keys'

* `vm_deploy_global_options:`
    `os_variant:` - Default: rhel7
    `network_options:` Default: type=direct,source=em3,source_mode=bridge
    `install_location:` Required: location of the rhel7 install location
    `os_disk_pool:` Default: home

You can specify the following 3 options for small, medium, and large VM's.

* `vm_deploy_small_options:`
   `vcpus:` - Default: 2
   `memory:`- Default: 4096
   `os_disk_size:` - Default: 20

* `vm_deploy_medium_options:`
  `vcpus:` - Default: 4
  `memory:`- Default: 8192
  `os_disk_size:` - Default 40

* `vm_deploy_large_options:`
  `vcpus:` - Default 8
  `memory:`- Default 16384
  `os_disk_size:` Default 60

Specify the name of VM's you wish to create.
The type is used by the role to build ansible in memory inventory and can be used
to flag a specific purpose that the VM is being deployed for.

* `vm_deploy_small_vms:`
  `name: "{{ vm_deploy_domain_name }}_small"`
  `type: small`

* `vm_deploy_medium_vms:`
  `name: "{{ vm_deploy_domain_name }}_medium"`
  `type: medium`

* `vm_deploy_large_vms:`
  `name: "{{ vm_deploy_domain_name }}_large"`
  `type: large`

Dependencies
------------

None

Example Playbooks
----------------
Required variables must be uncommented and or defined in defaults/main.yml.
This basic example would run with defaults and create 1 small, 1 medium, and 1 large VM.

```yaml

- hosts: vm_deploy_hypervisor
  roles:
    - role: vm_deploy
```

This example will create 3 medium VM's

```yaml

---
- hosts: vm_deploy_hypervisor
  roles:
    - role: vm_deploy
      vm_deploy_hypervisor: "hypervisor short name"
      vm_deploy_domain_name: 'domain'
      vm_deploy_ssh_key:
        "your ansible controller ssh key"
      vm_deploy_global_options:
        os_variant: rhel7
        network_options: type=direct,source=em3,source_mode=bridge
        install_location: "your rhel7 install location"
        os_disk_pool: home
        if_options: "--bootproto=dhcp --device=eth0 --ipv6=auto --activate"
      vm_deploy_medium_options:
        vcpus: 4
        memory: 8192
        os_disk_size: 40
      vm_deploy_medium_vms:
        - name: "{{ vm_deploy_domain_name }}_medium1"
          type: medium
        - name: "{{ vm_deploy_domain_name }}_medium2"
          type: medium
        - name: "{{ vm_deploy_domain_name }}_medium3"
          type: medium

```

License
-------

GPLv3

Author Information
------------------

Steve Murray Name <stmurray@redhat.com>
