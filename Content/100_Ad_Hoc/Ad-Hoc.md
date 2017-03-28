# Ad-Hoc Server Management

The term *`Ad-Hoc`* stands for `done for particular case in hand`. In Ansible, you can use _Ad-Hoc_ commands to get things done fast. Let us assume that you have to perform an one time task (ex. rebooting all the webservers) and you won't be performing that action often. This is where _Ad-Hoc_ commands come in to picture. Of course, you can write a complex shell script to do the same thing. But in Ansible, you can simply execute one line code which do the same thing. Its very simple and easy.

## Let's Try Ad-Hoc

Now we will try to ping the all the nodes using an ad-hoc command. Make sure you have ansible confguration file and inventory file in the current working directory (/workspace/Ansible-Training/Content/100_Ad_Hoc). Let's execute the following command.

```
ansible all -m ping
```









ansible all -m ping
ansible all -a hostname
ansible lb -a 'yum install -y ntp' --sudo
ansible lb -a 'yum remove -y ntp' --sudo
