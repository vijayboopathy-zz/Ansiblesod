ansible centos -m ping
ansible all -m hostname
ansible lb -a 'yum install -y ntp' --sudo
ansible lb -a 'yum remove -y ntp' --sudo
