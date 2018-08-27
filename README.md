IMS - Pre Migration for RHEL
============================

This role configures the Red Hat Enterprise Linux virtual machine for migration:

. It adds udev rules to enforce network interface naming
. It installs the guest agent for oVirt / Red Hat Virtualization

Role Variables
--------------

TBD

Example Playbook
----------------

The following playbook will register the virtual machine against a Satellite 6
server and enable the `rhelè7-server-rpms` repository. If no information is
provided in extra_vars, the playbook will assume that repositories are already
configured and will try to install the agent.

```yaml
---
- hosts: to_be_migrated
  vars:
    rhsm_config:
      server_hostname: "satellite.example.com"
      server_username: "admin"
      server_password: "secret"
      org_id: "my_organization"
    rhsm_repositories:
      - "rhel-7-server-rpms"
  roles:
    - role: ims.rhel_pre_migration
```

License
-------

GPLv3

Author Information
------------------

Fabien Dupont <fdupont@redhat.com>
