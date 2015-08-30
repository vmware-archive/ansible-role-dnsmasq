# ansible-role-dnsmasq

[Ansible](https://github.com/ansible/ansible) module for manipulating
DNS, DHCP and TFTP servers as altarnative to other such service
providers.

## Requirements

This role currently supports Debian/Ubuntu distros. The role will be in flux for a bit
as new features and capabilities are added. The general view is to allow a SuperXXX
server to also serve as the DHCP/DNS/TFTP and firewall/masquerade. The masquerade
would be between the internal servers (e.g., consul in CNA applications) and the external
world. Thus, SuperXXX sits in the middle and is available to developers and installers,
and can control the internal servers while providing them network access via NAT.

## Role Variables

Documentation here is under way . . . in flux as the vars get integrated into SuperVIO UI.

Whether to update the aptitude, yum, pacman or other package cache before running any install tasks.

## Example playbook

```
---
- name: setup net and dhcptftp roles
  hosts: net
  remote_user: ubuntu
  sudo: yes
  roles:
    - dnsmasq
  vars_files:
    - vars/uianswers.yml
```

# License and Copyright
 
Copyright 2015 VMware, Inc.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

## Author Information

This role was created in 2015 by [Tom Hite / VMware](http://www.vmware.com/).
