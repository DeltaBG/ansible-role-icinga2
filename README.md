# Ansible role for installing Icinga2 master

This role is WIP

##### Example host_vars when configuring a client: `inventories/icinga2/production/host_vars/client1.yml`

```
---
icinga2_client_os: Linux
icinga2_client_os_family: RedHat
icinga2_host_specific_services: |
  vars.http_vhosts["http"] = {
    http_uri = "http://localhost"
  }
  vars.disks["disk /"] = {
    disk_partitions = "/"
  }
  vars.notification["mail"] = {
    groups = [ "icingaadmins" ]
  }

# Individual thresholds are set via vars.
# Vars defined by generic services can be checked here: https://icinga.com/docs/icinga-2/latest/doc/10-icinga-template-library/
icinga2_client_custom_vars: |
  vars.http_enabled = true
  vars.swap_enabled = false
  vars.load_wload1 = 5
  vars.load_wload5 = 3
  vars.load_wload15 = 2
  vars.load_cload1 = 15
  vars.load_cload5 = 12
  vars.load_cload15 = 10
  vars.procs_warning = 300
  vars.procs_critical = 600
...
```
