# Modules - Batteries Included

- Come packaged
- Perform some tasks
- Types
  - Core Modules
  - Extra Modules
- Show available modules
  - From CLI
  - From Ansible site
- Actual Example

## Come Packaged

Modules are the scripts/plugins (written in python) `shipped` with Ansible. These modules are the main work horses of Ansible. All the action you do using Ansible, will be taken care of by one or more modules. Modules allow us to manage `independent components` of our infrastructure by describing its `desired state` and `properties`.

In the Ad-Hoc section, we had to play with couple of modules - `ping` and `command`

```
ansible all -m ping
ansible app -m command -a 'hostname'
```

Whenever you execute any ansible command, the modules are the actual code which gets executed in the remote machine. As said earlier, most of the modules take arguments.

## Types of Modules

Modules are of two types.

1. Core Modules
2. Extra Modules

At the time of writing this course, Ansible ships with more than 450 modules. The Core modules are the important modules which affect the functionality of Ansible directly. But the extra modules are add-ons to Ansible. Right now Ansible ships with both core and extra modules. In future releases, Ansible may only ship with core modules not the extra modules.

## List of Modules

Ansible has a command called `ansible-doc`. This command helps you to learn about any module you want.

If you want to learn about `yum` module, try

```
ansible-doc yum
```
Output will be like...

```
> YUM

  Installs, upgrade, removes, and lists packages and groups with the `yum' package manager.

Options (= is mandatory):

- conf_file
        The remote yum configuration file to use for the transaction.
        [Default: None]
- disable_gpg_check
        Whether to disable the GPG checking of signatures of packages being installed. Has an effect
        only if state is `present' or `latest'.
        (Choices: yes, no)[Default: no]
- disablerepo
        `Repoid' of repositories to disable for the install/update operation. These repos will not
        persist beyond the transaction. When specifying multiple repos, separate them with a ",".
        [Default: None]
[...]
```

Try to list all available modules using

```
ansible-doc -l
```

Output will be like...

```
a10_server                         Manage A10 Networks AX/SoftAX/Thunder/vThunder devices
a10_service_group                  Manage A10 Networks devices' service groups
a10_virtual_server                 Manage A10 Networks devices' virtual servers
acl                                Sets and retrieves file ACL information.
add_host                           add a host (and alternatively a group) to the ansible-playbook in-memory inventory
airbrake_deployment                Notify airbrake about app deployments
alternatives                       Manages alternative programs for common commands
apache2_mod_proxy                  Set and/or get members' attributes of an Apache httpd 2.4 mod_proxy balancer pool
apache2_module                     enables/disables a module of the Apache2 webserver
apk                                Manages apk packages
apt                                Manages apt-packages
apt_key                            Add or remove an apt key
apt_repository                     Add and remove APT repositories
apt_rpm                            apt_rpm package manager
asa_acl                            Manage access-lists on a Cisco ASA
asa_command                        Run arbitrary commands on Cisco ASA devices.
asa_config                         Manage Cisco ASA configuration sections
assemble                           Assembles a configuration file from fragments
assert                             Asserts given expressions are true
async_status                       Obtain status of asynchronous task
at                                 Schedule the execution of a command or script file via the at command.
atomic_host                        Manage the atomic host platform
atomic_image                       Manage the container images on the atomic host platform
[...]
```

You can also visit official [Ansible site](http://docs.ansible.com/ansible/list_of_all_modules.html) to see the documentation about modules.

## Examples

We are going to install `ntp` package on _app_ hostgroup. We did the same on _lb_ group. But here we will use `yum` module instead of `command` module. As said in the earlier section, It is always recommended to use dedicated module for managing entities. Only use `command` module if you are not able to find any dedicated module.

```
ansible app -m yum -a "name=ntp state=installed" --sudo
```

As said earlier, we just need to specify the state and property of the entity.

Output will be like...

```
node3 | SUCCESS => {
    "changed": true,
    "msg": "",
    "rc": 0,
    "results":
    [...]
    [...]
node4 | SUCCESS => {
    "changed": true,
    "msg": "",
    "rc": 0,
    "results":
    [...]
    [...]
node4 | SUCCESS => {
    "changed": true,
    "msg": "",
    "rc": 0,
    "results":
    [...]
```

Let's run this command again...

```
ansible app -m yum -a "name=ntp state=installed" --sudo
```

Will Ansible try to install ntp again?

```
node3 | SUCCESS => {
    "changed": false,
    "msg": "",
    "rc": 0,
    "results": [
        "ntp-4.2.6p5-10.el6.centos.2.x86_64 providing ntp is already installed"
    ]
}
node2 | SUCCESS => {
    "changed": false,
    "msg": "",
    "rc": 0,
    "results": [
        "ntp-4.2.6p5-10.el6.centos.2.x86_64 providing ntp is already installed"
    ]
}
node4 | SUCCESS => {
    "changed": false,
    "msg": "",
    "rc": 0,
    "results": [
        "ntp-4.2.6p5-10.el6.centos.2.x86_64 providing ntp is already installed"
    ]
}
```

No. This is why Ansible is `Idempotent`. The described state `installed` of the entity (`ntp`) matches with the actual state (`installed`). Whenever Ansible detects some changes in the state or property, then only it will try to bring the infrastructure component to the configured state. This property of Ansible is called `Idempotency`.

## Exercise

Try to create a user called `Dojo` with `no login` option enabled. Use this [link](http://docs.ansible.com/ansible/user_module.html) for reference.
