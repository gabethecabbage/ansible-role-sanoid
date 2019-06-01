# ansible-role-sanoid

An Ansible role to install and configure Sanoid/Syncoid for RHEL/Ubuntu

## Requirements

ZFS and systemd for remote hosts

Git installed on deployer

## Role Variables

```sanoid_git_tag```

Defaults to ```2.0.1```

Git tag of sanoid to pull on deployer and install on hosts.

```sanoid_datasets```

List of zfs datasets formatted as dictionaries of ```names``` and sanoid ```config```

Any sanoid configuration key value pairs for dataset can be set in the config dict and they will be set in the sanoid.conf file.

e.g.

```
sanoid_datasets:
  - name: "tank/prod/data"
    config:
      use_template: production
      recursive: yes
  - name: "tank/test/data"
    config:
      use_template: testing
      recursive: yes
```

```sanoid_templates```

List of sanoid snapshot schedule templates formatted as dictionaries of ```names``` and sanoid ```config```

Any sanoid configuration key value pairs for templates can be set in the config dict and they will be set in the sanoid.conf file.

e.g.

```
sanoid_templates:
  - name: production
    config:
      hourly: 24
      daily: 30
      monthly: 12
      autosnap: yes
      autoprune: yes
  - name: testing
    config:
      hourly: 0
      daily: 30
      monthly: 12
      autosnap: yes
      autoprune: yes
```

## Dependnecies

None

## Example Playbook

```
---
- hosts: fileservers
  roles:
    - role: ansible-role-sanoid
      become: true
      vars:
        sanoid_git_tag: 2.0.0
        sanoid_datasets:
          - name: "tank/prod/data"
            config:
              use_template: production
              recursive: yes
          - name: "tank/test/data"
            config:
              use_template: testing
              recursive: yes
        sanoid_templates:
          - name: production
            config:
              hourly: 24
              daily: 30
              monthly: 12
              autosnap: yes
              autoprune: yes
          - name: testing
            config:
              hourly: 0
              daily: 30
              monthly: 12
              autosnap: yes
              autoprune: yes
```
