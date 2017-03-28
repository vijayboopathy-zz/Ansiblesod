# Ansible Configuration

Ansible allows users to configure Ansible using Ansible configuration file. This file will exist in `/etc/ansible/ansible.cfg` by default. But this file is overridden in many cases. The precedence of Ansible configuration file goes like this...

1. ANSIBLE_CONFIG `environmental variable` (Highest Precedence)
2. ansible.cfg in `present` working directory
3. .ansible.cfg in `user home` directory
4. /etc/ansible/ansible.cfg (Least Precedence)

Let's take a look at our configuration file.

```
[defaults]
hostfile=inventory
remote_user=devops

[ssh_connection]
ssh_args = -o ControlMaster=no
```

## [default]

In the `[default]` section, as the name implies, we define some default values. In our case, we have a custom `hostfile` called `inventory`. The `remote_user` parameter helps us to define as what user Ansible is going to connect to the nodes. We have an existing user called *devops* in all the nodes. So we will use that.

## [ssh_connection]

By default, Ansible keeps ssh-connection alive for several minutes. We have some constraints with _Docker_, So we need to disable this feature using `ssh_args = -o ControlMaster=no`.
