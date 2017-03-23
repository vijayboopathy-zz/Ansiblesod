ansible centos -m ping
ansible all -m hostname
ansible centos -a 'yum install -y ntp' --sudo
ansible centos -a 'yum remove -y ntp' --sudo
