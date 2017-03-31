# Ad-Hoc Server Management

The term `Ad-Hoc` stands for `done for particular case in hand`. In Ansible, you can use _Ad-Hoc_ commands to get things done fast. Let us assume that you have to perform an one time task (ex. rebooting all the webservers) and you won't be performing that action often. This is where _Ad-Hoc_ commands come in to picture. Of course, you can write a complex shell script to do the same thing. But in Ansible, you can simply execute one line code which do the same thing. Its very simple and easy.

## Let's Go Ad-Hoc

Now we will try to ping the all the nodes using an ad-hoc command. Make sure you have ansible confguration file and inventory file in the current working directory (/workspace/Ansible-Training/Content/100_Ad_Hoc). Let's execute the following command.

```
ansible all -m ping
```

Your output will be...

```
node1 | SUCCESS =>{                                    
    "changed": false,
    "ping":"pong"
}
node2 | SUCCESS =>{                                    
    "changed": false,
    "ping":"pong"
}
node3 | SUCCESS =>{                                    
    "changed": false,
    "ping":"pong"
}
node4 | SUCCESS =>{                                    
    "changed": false,
    "ping":"pong"
}
```

The above mentioned command is like `"Hello-World"` for Ansible. This command establishes the following things.

* You have installed and configured Ansible correctly
* Passwordless SSH is working
* All the nodes are up

Now let us talk about the construction of the command.

* *ansible*
    * This part invokes `/usr/bin/ansible` command. This is how you start an Ad-Hoc command.

* *all*
    * After invoking ansible, we need to specify what nodes are we going to target. In our case, we use `all` parameter which represents all the nodes listed in our inventory. This can be a specific node or host group as well.

* *-m*
    * `-m` enables us to use `modules`. Modules are prewritten code that helps us to get job done quickly. We will cover more about modules in the second chapter.

* *ping*
    * We are using `ping` module in this ad-hoc command.

  From the above example, we have learnt that, the construction of an ad-hoc command look like,

  ```
  ansible <target host/hostgroup> -m <module_name>
  ```

## More Ad-Hoc

Now we have a basic understanding of what is an ad-hoc command and how do we use it. Let us try to execute more ad-hoc command to get the hang of it.

### Get Hostname

This time we are going to try something different. Let us target the `app` host group and get the hostnames of the machines in that group.

```
ansible app -m command -a "hostname"
```

Have you noticed something different here? Good. We are targeting app host group by mentioning `app` after ansible command. `command` denotes that we are using command `module`. But, What is `-a`?. In Ansible, most of the modules take `arguments`. We pass _arguments to modules_ by using -a option. In this case, command module takes `all the commands we use in cli`. Now let's see the output of this command.

```
node3 | SUCCESS | rc=0 >>
node3.codespaces.io

node2 | SUCCESS | rc=0 >>
node2.codespaces.io

node4 | SUCCESS | rc=0 >>
node4.codespaces.io
```

Since we only targetted `app` host group, we have got the output for those machines alone.

You can get the same output by running,

```
ansible app -a hostname
```

### Why are we not using a module here?

`Command` module is one of the most used module for ad-hoc commands. It is the default module. So Ansible _doesn't_ require you to specify module name for command module.

## Installing packages

Till now we have not done anything of much importance. Let's try to install `ntp` package in our `lb` host group. From hereon, I am not going to explain about the construct of the command.

```
ansible lb -a "yum install -y ntp"
```

Let us see the output.

```
node1 | FAILED | rc=1 >>
Loaded plugins: fastestmirror, ovlovl: Error while doing RPMdb copy-up:
[Errno 13] Permission denied: '/var/lib/rpm/.rpm.lock'
You need to be root to perform this command.
```

Oops... We have got our first error. From the output, we can learn that user `devops` (remember the user in ansible configuration?) does not have permission to install the package. This often is the case. So how do we fix this error? It can be easily fixed by providing `--sudo` option at the end of the command.

```
ansible lb -a "yum install -y ntp" --sudo
```

```
...
Installed:
  ntp.x86_64 0:4.2.6p5-10.el6.centos.2

Dependency Installed:
  libedit.x86_64 0:2.11-4.20080712cvs.1.el6
  ntpdate.x86_64 0:4.2.6p5-10.el6.centos.2

Complete!
```

But using ad-hoc for installing packages and doing configuration management is `not recommended`. Ansible's power lies in playbooks and users should stick with writing playbooks for configuration management. Ad-Hoc is only for `redundant one time taks`.
Do not make it as a habit of only using Ansible ad-hoc commands.

## Reference

We have almost covered all the aspects of Ansible Ad-Hoc commands. For further advanced reading, use this [link](http://docs.ansible.com/ansible/intro_adhoc.html).
