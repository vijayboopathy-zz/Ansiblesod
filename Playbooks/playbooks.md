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

    - name: Install OpenJDK8
      yum:
        name: java-1.8.0-openjdk
        state: present
