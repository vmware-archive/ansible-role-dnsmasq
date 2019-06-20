# ansible-role-dnsmasq

[Ansible](https://github.com/ansible/ansible) module for manipulating
DNS, DHCP and TFTP servers as alternative to other such service
providers.

## Requirements

This role currently contains VMware ESXi dependent variable, all of which
will be moved to another role very soon.

This role currently supports Debian/Ubuntu distros. The role will be in flux
for a bit as new features and capabilities are added. The general view is to
allow a server, for instance a [Chaperone](https://github.com/vmware/chaperone)
UI web server, to also serve as the DHCP/DNS/TFTP and firewall/masquerade. The
masquerade would be between the internal servers (e.g., consul in CNA
applications) and the external world. Thus, the server this role sets up sits in
the middle and is available to developers and installers, and can control the
internal servers while providing them network access via NAT.

## Role Variables

```yaml
# Default domain name
dnsmasq_domain_name: "corp.local"

# Interfaces that should supply DNS resolution, but not DHCP
# Exapmle: dnsmasq_only: [ "eth2", "eth5" ]
dnsmasq_dns_only: []

# 'External' interfaces (implicitly *not* DHCP serving), often one use for external NAT
# Format: - { search: 'corp.local vmware.com',  broadcast: '192.168.0.255', prefixbits: "24", netmask: '255.255.255.0',  dns1: '10.148.20.5', dns2: '10.148.20.6', address: '192.168.0.2', device: 'eth3', type: 'static', gateway: '192.168.0.1', name: "public"}
dnsmasq_external_interfaces: []

# external interface (for NAT, this is generally a DHCP client based NIC).
dnsmasq_dhcp_interfaces:
  - eth0

# Ranges to serve for DHCP IP addresses not otherwise statically assigned.
# Format:  - { Interface: eth1, Range: "172.16.69.32,172.16.69.254", Netmask: "255.255.255.0", Lease: 1440, DNS: "172.16.69.1" }
dnsmasq_dhcp_ranges: []

# For each net device that DHCP is desired, set the network up as a static router interface.
# Format: - {  device: "eth0", address: "192.168.10.2", prefixbits: "24", netmask: "255.255.255.0", broadcast: "192.168.10.255",  range: "192.168.10.3,static", lease: 1440, dns: "192.168.10.2", name: "management" }
dnsmasq_dhcp_networks: []

# Set static reservations here for assuring specific serves addresses
# These need not be in the ranges above.
# Format: - { hostname: "nfs-01a.corp.local", address: "192.168.110.11", mac: "00:50:56:03:3e:b0" }
dnsmasq_dhcp_reservations: []

# hosts not otherwise setup by DHCP requestors we want in the domain.
#Format:  - { ipv4: "10.150.111.233", host: "registry.corp.local" }
dnsmasq_additional_hosts: []

# alias (cname) records
# Format:  - { host: "router.corp.local", target: "chaperone-ui.corp.local" }
dnsmasq_cnames: []

# The path to 'additional' dnsmasq configurations
dnsmasq_etc_path: "/etc/dnsmasq.d"

# the path to the the resolv file we want to use
dnsmasq_resolv_file: "{{ dnsmasq_etc_path }}/dnsmasq.resolv"

# the path to the the resolv file we want to use
dnsmasq_dhcp_hosts_dir: "{{ dnsmasq_etc_path }}/dhcp-hosts"

# Enable tftp services
dnsmasq_enable_tftp: False
dnsmasq_tftp_unique_root: False
dnsmasq_tftp_web_port: 80
dnsmasq_tftp_web_loglevel: "INFO"
dnsmasq_tftp_images_dir: "/tftp/pxeboot/images"
dnsmasq_tftp_hostname: "tftp.corp.local"
dnsmasq_tftp_kickstart_dir: "/tftp/webroot/KS"

# the environment -- includes the tasks to setup the tftp services
# for specified environment
dnsmasq_tftp_env: 'esxi'

# TODO: Remove the remainder since specific to ESXi
# --- ESXi specific info
# ESXi PXE boot installer info
esxi_tftp_esxi_version: "6.0.0-24945856"
esxi_tftp_iso: "VMware-VMvisor-Installer-{{ esxi_tftp_esxi_version }}.x86_64.iso"
esxi_tftp_iso_mountpoint: "/tmp/tftp_iso_mount"

# Format: root,user2,myname
esxi_full_access_users: root

# NFS stores to set as shared storage on ESXi hosts.
# Format:  - { ipv4: "nfs-01a.corp.local",  export: "/exports/esxi", vol_name: "nfs-01a" }
esxi_nfs_stores: []

# Setup for firewall services
esxi_firewall_enabled: "yes"
esxi_firewall_services: "syslog sshClient ntpClient updateManager httpClient netdump"

# List any vmnics to setup for DHCP
esxi_dhcp_vmnics:
  - vmnic0

# List any vmnics to setup for static IP (examples included)
# Format:  - { name: "vmnic0", hostname: "esx-oob-01a.corp.local",  address: "192.168.110.50", netmask: "255.255.255.0", gateway: "192.168.110.1", dns1: "192.168.110.1" }
esxi_static_vmnics: []

# List any vSwitches and portgroups to setup
# Format:
esxi_vswitches:
  - { vsname: "vSwitch0", pgname: "Management" }

# Set the kickstart info: target is the target environment (and thus
# has a directory in templates/tftp/; and template is the template
# to use for generating the kickstart (see templates/tftp).
esxi_kickstart_target: "vpod"
esxi_kickstart_template: "ESX_VPOD_KS.CFG.j2"
```

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

Copyright 2015 VMware, Inc.  All rights reserved.

SPDX-License-Identifier: Apache-2.0 OR GPL-3.0-only

This code is Dual Licensed Apache-2.0 or GPLv3

## Author Information

This role was created in 2015 by [Tom Hite / VMware](http://www.vmware.com/).
