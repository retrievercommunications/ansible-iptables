# iptables

Installs and configures iptables.

## Source

Originally forked from: [GitHub][github_link]

Forked version: v3.0.1-a

## Requirements

- Ansible 2.0 or higher.
- Root privileges, e.g. `become:yes`

## Dependencies

None

## Variables

| Name | Description | Default |
|------|-------------|---------|
| `iptables_ssh_source_address` | The default source subnet for the SSH iptables INPUT rule. | `0.0.0.0\0` |
| `iptables_ssh_port` | The default TCP port for the SSH iptables INPUT rule. | `22` |
| `iptables_filter_input_policy` | IPv4 default filter input policy. | `drop` |
| `iptables_filter_forward_policy` | IPv4 default filter forward policy. | `drop` |
| `iptables_filter_output_policy` | IPv4 default filter output policy. | `accept` |
| `iptables_filter_rules` | Array of filter rules represented as hashes. | `[]` |
| `iptables_nat_prerouting_policy` | IPv4 default nat prerouting policy. | `accept` |
| `iptables_nat_input_policy` | IPv4 default nat input policy. | `accept` |
| `iptables_nat_output_policy` | IPv4 default nat output policy. | `accept` |
| `iptables_nat_postrouting_policy` | IPv4 default nat postrouting policy. | `accept` |
| `iptables_nat_rules` | Array of nat rules represented as hashes. | `[]` |
| `iptables6_filter_input_policy` | IPv6 default filter input policy. | `drop` |
| `iptables6_filter_forward_policy` | IPv6 default filter forward policy. | `drop` |
| `iptables6_filter_output_policy` | IPv6 default filter output policy. | `accept` |
| `iptables6_nat_prerouting_policy` | IPv6 default nat prerouting policy. | `accept` |
| `iptables6_nat_input_policy` | IPv6 default nat input policy. | `accept` |
| `iptables6_nat_output_policy` | IPv6 default nat output policy. | `accept` |
| `iptables6_nat_postrouting_policy` | IPv6 default nat postrouting policy. | `accept` |
| `iptables_additional_modules` | A list of additional modules to load. **Applies to RedHat systems only** | `[]` |

## Example Playbook

Install and configure iptables to allow ICMP and OpenSSH

```yaml
- hosts: all
  roles:
    - iptables
```

Install and configure iptables to disallow ICMP, allow OpenSSH and HTTP

```yaml
- hosts: all
  vars:
    iptables_filter_rules:
      - chain: input
        protocol: tcp
        source_address: 0.0.0.0/0
        destination_port: 22
        comment: OpenSSH
        target: accept
      - chain: input
        protocol: tcp
        source_address: 0.0.0.0/0
        destination_port: 80
        comment: HTTP
        target: accept
  roles:
    - iptables
```

Install and configure iptables with a port forward rule for HTTP

```yaml
- hosts: all
  vars:
    iptables_filter_rules:
      - chain: input
        protocol: tcp
        source_address: 0.0.0.0/0
        destination_port: 80
        comment: HTTP
        target: accept
    iptables_nat_rules:
      - chain: prerouting
        protocol: tcp
        destination_port: 80
        target: dnat
        to_destination: 192.168.1.54
        to_port: 8080
  roles:
    - iptables
```

### License

BSD

### Original Author Information

Kevin Brebanov

[github_link]:           https://github.com/retrievercommunications/ansible-iptables