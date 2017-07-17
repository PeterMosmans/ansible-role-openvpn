Ansible Role: openvpn
=========

Build status for this
role:
[![Build Status](https://travis-ci.org/PeterMosmans/ansible-role-openvpn.svg)](https://travis-ci.org/PeterMosmans/ansible-role-openvpn)

This role installs and configures (hardens) an OpenVPN service, including a
firewall (ufw). It does *not* generate keys and certificates, but it can and
will deploy those.


Requirements
------------

You'll need a Certificate Authority certificate (`ca.crt`), a server private key
(`server.key`) and corresponding certificate (`server.crt`), and a
Diffie-Hellman parameter file (`dh2048.pem`) for this role.

It is highly recommended to generate those 'offline', and not use tools like
e.g. `easy-rsa` to generate those on the OpenVPN server itself.

The role will **fail** without those files.


Role Variables
--------------

Available variables are listed below, along with default values

**openvpn_interface**: Default networking interface. The default value can be
found in `defaults/main.yml`:

```
openvpn_interface: enp0s3
```


**openvpn_ipv6_server**: Private ipv6 server address. By default, it is
not configured, and ipv6 will not be used/tunneled. If set, then **all** ipv6 traffic
will be tunneled over the OpenVPN tunnel.

Example:
```
openvpn_ipv6_server: fdaa:bbbb:cccc:dddd:eeee:/64

```


**openvpn_path**: Path where OpenVPN expects its configuration. The default
value can be found in `defaults/main.yml`:

```
openvpn_path: /etc/openvpn
```


**openvpn_port**: Port where OpenVPN server will listen on. The default value
can be found in `defaults/main.yml`:

```
openvpn_port: 1194
```


**openvpn_proto**: Protocol being used by OpenVPN. The default value can be
found in `defaults/main.yml`:

```
openvpn_proto: udp
```

### Optional:

**openvpn_static_key**: If this parameter is defined, then a static key will be
used by the server. The static key will be expected as `tls-auth.key`, and
should be accessible by this role (e.g. by locating it in the `files` folder).


**openvpn_use_crl**: If this parameter is defined, then a certificate
revocation list (CRL) will be deployed to the server, and the server will be
configured to use it. The CRL will be expected as `crl.pem`, and should be
accessible by this role (e.g. by locating it in the `files` folder).


## Templates

The openvpn server configuration can be found under
``templates/server.conf.j2``. All variables mentioned earlier are automatically
being templated.

Dependencies
------------

Note that the role expects keys, certificates and Diffie-Hellman parameters in
the/a `files` directory, e.g. from where this playbook is invoked. Currently the
names of the files are hardcoded in ``server.conf.j2``:

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
