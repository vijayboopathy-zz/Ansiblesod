# Inventory

## What is an Inventory?

As we know, Ansible is a Configuration Management system which performs actions against a set of systems. These set of machines are defined in a file called `Inventory`. Inventory file follows the `ini` format.

### Example Inventory file

```
[loadbalancers]
lb1
lb2

[webservers]
web1
web2
web3

[messagequeue]
redis1
redis2

[databases]
db1
db2
db3
```

## Why need a custom inventory?

The default inventory of Ansible is in `/etc/ansible/hosts`. However, it is not a good idea to use this hosts file. Because, inventory is one of the key part of Ansible and if we lose the Ansible Control Machine, we will lose the inventory. So it is a good idea to create your own custom inventory file and use some `Source Code Management` system (ex. Github) to keep your Ansible code and inventory file safe.

## Anatomy of an Inventory file

### Hosts

Let's break the above mentioned inventory into multiple parts. An inventory file consists of hosts or group of hosts. If your inventory simply consists of some hosts, it will look like this...

```
host1
host2
host3
host4
host5
```

Here, each entry represents one machine. These entries can be the `hostnames`, `IP-Addresses` or the `FQDN` of the machines.

### Host Groups

If you have a big infrastructure, your inventory file will easily become hard to manage. We can put multiple machines in a group based on the role (or any other characteristics). This group is called `Host Group`. Let us assume that, we have a three tier architecture which has webservers in the front end, application server in the middle and databases in the backend.

If we don't have the groups of machines here, It will become hard for the sys-admin to manage.

#### Example of Host Groups

```
[web]
web1
web2

[app]
app1
app2
app3

[db]
db1
db2
```

The string inside brackets `[]` is called a `Group Name`. This group name represents the list of hosts defined under it.

To get started with Ansible, the above given information is more than enough. For more `advanced reading` on inventory file, read further.

## Aliases

Assume that your machines doesn't have hostnames and you want to call these machines using an alias rather IPs. This can be done by using `ansible_host` flag.

```
[web]
web1 ansible_host=192.168.0.47
```

Here, web1 is the alias for the machine with the 192.168.0.47 IP. Whenever you invoke Ansible to perform some action against `web1`, it will get resolved to `192.168.0.47`

## Ports

If your machines does not run SSH Daemon on default port(22), you can define a custom port to which Ansible can connect. You can do this by enabling `ansible_port` flag in the inventory.

```
[db]
db1 ansible_port=3444
```

Whenever you invoke Ansible to do some operations against the machine `db1`, it will not use the default port. Instead it will use the port `3444`.

## Our Inventory

Our inventory file is going to look like this. It consists of 4 nodes. 1 node will act as a load balancer and other three as appication servers. 

```
[nodes]
node1
node2
node3
node4

[lb]
node1

[app]
node2
node3
node4
```

## Reference

The official documentation for Ansible inventory is full of examples and it covers more topic on Inventory. For further references about inventory, please visit this [link](http://docs.ansible.com/ansible/intro_inventory.html).
