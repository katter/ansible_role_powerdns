# PowerDNS

Role to install and configure PowerDNS (minimal as of yet)

# Requirements


## Role Variables

The role accepts variables as PowerDNS accepts configurable settings as defined here: https://doc.powerdns.com/md/authoritative/settings/
Add a leading "powerdns_" and replace dash with underscore.
Example: To allow recursion from network 192.168.1.0 only you will set the var
`powerdns_allow_recursion: 192.168.1.0/24` which will set `allow-recursion=192.168.1.0/24` in your powerdns config file.

Additional Variables:
| Var | Description | Required? |
| --- | ----------- | --------- |
| `powerdns_package_url`      | the location to fetch the PoweDNS package from | Yes |
| `powerdns_package_name`        | the name of the file to fetch for install | Yes |


## Example Playbook

```
---
- hosts: all
  roles:
    - role: powerdns
      powerdns_carbon_server: 192.168.1.45
      powerdns_log_dns_queries: yes

```
