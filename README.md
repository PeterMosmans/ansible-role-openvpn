Ansible Role: openvpn
=========

Build status for this role: [![Build Status](https://travis-ci.org/PeterMosmans/ansible-role-openvpn.svg)](https://travis-ci.org/PeterMosmans/ansible-role-openvpn)

This role installs and configures an OpenVPN service, including a firewall (ufw). It does *not* generate keys and certificates, but it can and will deploy those.

Requirements
------------

None.

Role Variables
--------------

Available variables are listed below, along with default values

# Default networking interface
**openvpn_interface**: Default networking interface. The default value can be found in `defaults/main.yml`:
```
openvpn_interface: enp0s3
```


**openvpn_path**: Path where OpenVPN expects its configuration. The default value can be found in `defaults/main.yml`:
```
openvpn_path: /etc/openvpn
```


**openvpn_port**: Port where OpenVPN server will listen on. The default value can be found in `defaults/main.yml`:
```
openvpn_port: 1194
```


**openvpn_proto**: Protocol being used by OpenVPN. The default value can be found in `defaults/main.yml`:
```
openvpn_proto: udp
```

## Templates

The openvpn server configuration can be found under ``templates/server.conf.j2``. All variables mentioned earlier are automatically being templated.

Dependencies
------------

Note that the role expects keys, certificates and Diffie-Hellman parameters in the/a `files` directory, e.g. from where this playbook is invoked. Currently the names of the files are hardcoded in ``server.conf.j2``:
```
ca.crt
server.crt
server.key
dh2048.pem
```

Example Playbook
----------------
```
- hosts: all
  become: yes
  become_method: sudo
  roles:
  - role: PeterMosmans.openvpn
```
This example will install and configure OpenVPN.



License
-------

GPLv3


Author Information
------------------

Created by Peter Mosmans.
