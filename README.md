iptables
========

[![Ansible Role](https://img.shields.io/ansible/role/3393.svg)](https://galaxy.ansible.com/list#/roles/3393)

Installs and configures iptables.

Requirements
------------

This role requires Ansible 1.4 or higher.

Role Variables
--------------

| Name                        | Default                                                                       | Description                                       |
|-----------------------------|-------------------------------------------------------------------------------|---------------------------------------------------|
| iptables_input_policy       | "DROP"                                                                        | Default input policy                              |
| iptables_forward_policy     | "DROP"                                                                        | Default forward policy                            |
| iptables_output_policy      | "ACCEPT"                                                                      | Default output policy                             |
| iptables_icmp_enabled       | true                                                                          | Enable/disable ICMP                               |
| iptables_rules              | [{protocol: tcp, source_addresses: 0.0.0.0/0, port: 22, comment: "OpenSSH" }] | Array of firewall rules represented as hashes     |
| iptables_port_forward_rules | []                                                                            | Array of port forward rules represented as hashes |

Dependencies
------------

CentOS:
  - kbrebanov.selinux

Example Playbook
----------------

Install and configure iptables to allow ICMP and OpenSSH
```
- hosts: all
  roles:
    - kbrebanov.iptables
```

Install and configure iptables to disallow ICMP, allow OpenSSH and HTTP
```
- hosts: all
  vars:
    iptables_icmp_enalbed: false
    iptables_rules:
      - protocol: tcp
        source_addresses: 0.0.0.0/0
        port: 22
        comment: "OpenSSH"
      - protocol: tcp
        source_addresses: 0.0.0.0/0
        port: 80
        comment: "HTTP"
  roles:
    - kbrebanov.iptables
```

Install and configure iptables with a port forward rule for HTTP
```
- hosts: all
  vars:
    iptables_rules:
      - protocol: tcp
        source_addresses: 0.0.0.0/0
        port: 80
        comment: "HTTP"
    iptables_port_forward_rules:
      - protocol: tcp
        port: 80
        to_ip: 192.168.1.54
        to_port: 8080
  roles:
    - kbrebanov.iptables
```

License
-------

BSD

Author Information
------------------

Kevin Brebanov
