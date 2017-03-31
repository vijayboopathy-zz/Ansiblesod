# Playbooks

- What is a Playbook?
- YAML?
- Construction of a playbook (Single Play - Install OpenJDK)
- Syntax Check
- Dry run
- Actual run
- Multiple plays
- limit

## What is a Playbook?

* The true power of Ansible lies in Playbooks.
* Playbooks help you to write `infrastructure as code`.
* Playbooks help you to abstract code from data. By code I mean, state and property of entities. This code can be used anywhere not only in your infrastructure. Only thing that changes is data. Thus you can create and recreate the same infrastructure without facing any challenges.

Let us take a look at an example playbook. Don't try to understand it. This is just to see how a playbook will look like.

```
---
  - name: Base Configurations for ALL hosts
    hosts: all
    become: true
    tasks:
      - name: create admin user
        user: name=admin state=present uid=5001

      - name: remove dojo
        user: name=dojo  state=present

      - name: install tree
        yum:  name=tree  state=present

      - name: install ntp
        yum:  name=ntp   state=present

      - name: start ntp service
        service: name=ntp state=started enabled=yes
```

## YAML

Ansible uses `YAML` as its automation language. YAML stands `Yet Another Markup Language`. It is an easy language to pick up and understand. One important point is, YAML *does not* take `tabs` as input. So always use `spaces`.

### YAML Guide

#### Begin and End tag

Every YAML file starts with a begining tag `---` and ends with an end tag `...`. These tags are not mandatory to be used in playbooks but it is always a `good practice` to use them.

```
---
- name:
  hosts:
  become:
  tasks:
...
```

#### Lists

Every playbook has `lists of items`. These items can be *tasks*, *hosts* or other options. In the below given example, tasks has a list of modules.

```
---
- name:
  hosts:
  become:
  tasks:
    - copy
    - template
    - yum
    - service
```
#### Dictionaries

Every item in the lists has a dictionary in it. In Ansible terms, these key value pairs are the arguments taken by the modules.

```
---
- name:
  hosts:
  become:
  tasks:
    - copy: src=files/test.txt dest=/home/devops/
    - yum: name=vim state=installed
    - service: name=httpd state=started
```


## Construction of a Playbook

* Every playbook has one or more `plays`.
* Each play has `4` important parts

Part    |   Description
--------|---------------
`Name`  |  Name of the play
`Hosts` |  Targeted Hosts/Hostgroups
`Become`|  Run as a privileged user
`Tasks` |  Actual operations to be performed


## Tasks

* _Tasks_ are the part of playbook where we define desired state/action to be performed in the infrastructure.
* Every `Task` invokes `one module` to take some `action`.
* You can refer to any above given examples to get an idea about _tasks_.

### Recommended Task Definition

* Even though you can define one task in one line, it is recommended to have `multi-line tasks`. It improves the readability and manageability of the playbooks.
* Let us refactor the first example

```
---
- name: Base Configurations for ALL hosts
  hosts: all
  become: true
  tasks:
    - name: create admin user
      user:
        name=admin
        state=present
        uid=5001

    - name: remove dojo
      user:
        name=dojo
        state=present

    - name: install tree
      yum:
        name=tree
        state=present

    - name: install ntp
      yum:
        name=ntp
        state=present

    - name: start ntp service
      service:
      name=ntp
      state=started
      enabled=yes
```

* This is the recommended way of writing tasks in playbooks.
* This playbook has only one play in it. But a playbook can contain more than one play.

## Problem Statement

* From here on we will focus on one problem statement till the end of the course. We are going to
    * Install and Configure Tomcat Application server
    * Set up a load balancer for our app servers. We will use `nginx` as our load balancer
    * Deploy applications to the application servers.
* We will break down this problem in to multiple parts and which will be implemented by various features of Ansible.

## Our First Playbook

* Now let us create our first playbook.
* The first step to implement a solution is to install the dependency packages of Tomcat.

```
---
- name: Install and Configure OpenJDK
  hosts: app
  become: yes

  tasks:
    - name: Enable EPEL repository
      yum:
        name: epel-release
        state: present
        update_cache: yes

    - name: Install OpenJDK7
      yum:
        name: java-1.7.0-openjdk
        state: present

```

## Exercise

* Create a playbook which contains `two plays`,
  * The first play will target hostgroup `app`,
    * Create a user and group called `mojo`,
    * Check the `free memory` in the machines,
    * Execute a echo command and says `Ansible is awesome :D`.
  * The second play will target hostgroup `lb`,
    * Copy a file from `pwd` to `/home/devops`,
    * Delete the user and group `mojo`,
    * Install the package `cowsay`.
