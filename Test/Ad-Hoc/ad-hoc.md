ansible all -m ping
ansible all -a hostname
ansible lb -a 'yum install -y ntp' --sudo
ansible lb -a 'yum remove -y ntp' --sudo
