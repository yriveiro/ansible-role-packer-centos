# Ansible Role: Packer CentOS Configuration for Vagrant VirtualBox

[![Build Status](https://travis-ci.org/yriveiro/ansible-role-packer-centos.svg?branch=master)](https://travis-ci.org/yriveiro/ansible-role-packer-centos)

This role configures CentOS (either minimal or full install) in preparation for it to be packaged as part of a .box file for Vagrant/VirtualBox deployment using [Packer](http://www.packer.io/).

This role is only focused on VirtualBox.

## Requirements

Ansible must be installed via a shell provisioner. Packer .json template would be something like:

```json
"provisioners": [
  {
    "type": "shell",
    "execute_command": "echo 'vagrant' | {{.Vars}} sudo -S -E bash '{{.Path}}'",
    "script": "scripts/ansible.sh"
  },
  {
    "type": "ansible-local",
    "playbook_file": "ansible/main.yml",
    "role_paths": [
      "./ansible-roles/yriveiro.packer-centos",
    ]
  }
],
```

The files should contain, at a minimum:

**scripts/ansible.sh**:

```sh
#!/bin/bash -eux
# Add the EPEL repository, and install Ansible.
rpm -ivh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
yum -y install ansible python-setuptools
```

**ansible/main.yml**:

```
---
- hosts: all
  sudo: yes
  gather_facts: yes
  roles:
    - yriveiro.packer-centos
```

## Role Variables

None.

## Dependencies

None.

## Example Playbook

```
P
```

## License

MIT

## Author Information

This role is highly inspired in a role created by [Jeff Geerling](http://jeffgeerling.com/), author of [Ansible for DevOps](http://ansiblefordevops.com/).
