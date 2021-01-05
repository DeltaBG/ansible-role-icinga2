# Ansible role for installing and configuring Icinga2 master

## Requirements

This role depends on the extra plugin from `Galaxy deltabg.extended_facts`.
Run `ansible-galaxy install -r DeltaBG.icinga2/requirements.yml` before using this role.

## Prerequisites

* At least one Icinga2 master host and one Icinga2 client must be defined in your inventory file, otherwise the roles will fail.
* Role deltabg.icinga2_client must be present

## Supported OS

* CentOS 8

### Example inventory in .INI format

```
[icinga_master]
icinga2-master1 ansible_host=1.2.3.4

[icinga_clients]
client1 ansible_host=1.2.3.5
```

### Example playbook `playbooks/icinga2.yml`

```
---
- name: Gather facts from clients
  hosts: icinga_clients
  pre_tasks:
    - name: Ensure smartmontools is present
      package:
        name: smartmontools
        state: present
      tags: configure
  tasks:
    - deltabg.extended_facts.extended_facts:
      tags: configure

- name: Configure clients on Icinga2 master
  hosts: icinga_master
  roles:
    - deltabg.icinga2

- name: Install Icinga2 on clients
  gather_facts: no
  hosts: icinga_clients
  roles:
    - deltabg.icinga2_client
...
```

### Icinga2 plugins

There are number of plugins which are installed on the Icinga2 master and/or Icinga2 clients. Some of them use the `deltabg.extended_facts` plugin to automatically configure each host, others need manual configuration. Manual configuration is done via host_vars. Please note, that at a minumim, it is not neccessary to create host_vars, you are needed to place your Icinga2 client in the inventory file only. Here is a list of installed plugins, where they are installed and whether or not they need manual configuration (meaning they are configured using facts from the deltabg.extended_facts plugin) via host_vars:

| Plugin name                        | Location | Configuration |
|------------------------------------|----------|---------------|
| ipmi_sensor_v3                     | Master   | Manual        |
| ping4                              | Master   | Automatic     |
| ping6                              | Master   | Automatic     |
| ssh                                | Agent    | Automatic     |
| http_vhost                         | Agent    | Manual        |
| disk                               | Agent    | Automatic     |
| icinga                             | Agent    | Automatic     |
| load                               | Agent    | Automatic     |
| procs                              | Agent    | Automatic     |
| swap                               | Agent    | Automatic     |
| users                              | Agent    | Automatic     |
| mysql                              | Agent    | Automatic     |
| mysql_health_slave_replication_lag | Agent    | Manual        |
| mysql_health_slave_io_running      | Agent    | Manual        |
| mysql_health_slave_sql_running     | Agent    | Manual        |
| check_rbl                          | Agent    | Automatic     |
| check_raid                         | Agent    | Automatic     |
| check_smart_pl                     | Agent    | Automatic     |
| iostats                            | Agent    | Automatic     |

#### Tips and tricks

* In order to configure and install single client, you can use the `--limit` option when running your playbook. Example usage: `ansible-playbook playbooks/icinga2.yml -i inventories/hosts.ini --limit 'client1'`
