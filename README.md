IMS - Pre Migration for RHEL
============================

This role configures the Red Hat Enterprise Linux virtual machine for
migration:

- It adds udev rules to enforce network interface naming
- It installs the guest agent for oVirt / Red Hat Virtualization

Role Variables
--------------

The role behaviour can be influenced via some variables that are mainly
wrappers for the module used in the role. ***By default, no variable is set
and the role will expect that RPM repositories are already setup on the
host***.

If, repositories are not configured yet on the host, one can use the following
variables to configure them. Three configuration methods can be used: RHN
(based on Red Hat Satellite 5 / Spacewalk), RHSM (based on Red Hat Satellite
6 / Katello) and `.repo` files pointing to local repositories.

- **RHN** - The configuration relies on two variables:
  - `rhn_config` configures the system to use Red Hat Satellite 5 / Spacewalk.
    It is a hash whose keys are the parameters of the
    [`rhn_register`](https://docs.ansible.com/ansible/latest/modules/rhn_register_module.html)
    module.

  - `rhn_channels` configures the software channels to be used by the host.
    If an activation key has been used with `rhn_config`, it may be useless.

- **RHSM** - The configuration relies on two variables:
  - `rhsm_config` configures the system to use Red Hat Satellite 6 / Katellois.
    It is a hash whose keys are the parameters of the
    [`redhat_subscription`](https://docs.ansible.com/ansible/latest/modules/redhat_subscription_module.html)
    module.

- **Ad-hoc repositories** - The configuration relies on a single variable:
  - `yum_repositories` is an array of hashes, each hash allowing being a
    wrapper for the
    [`yum_repository`](https://docs.ansible.com/ansible/latest/modules/yum_repository_module.html)
    module.


Example Playbook
----------------

The following playbook will register the virtual machine against a Satellite 6
server and enable the `rhel-7-server-rpms` repository. If no information is
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
