## LAB: Targeting Specific Nodes

This scenario shows:
- how to target specific nodes

### Prerequisite

- You should have a look following lab, nodes that are created in that LAB, are using in ansible commands
  - [LAB: Multipass-SSH Configuration (Create Ansible Test Environment)](https://github.com/omerbsezer/Fast-Ansible/blob/main/Multipass-SSH-Configuration.md)

### Steps

- Update inventory file.
- Make groups by giving names with '[]'.

``` 
[web_servers]
172.21.79.85

[database_servers]
172.21.76.101
``` 

![image](https://user-images.githubusercontent.com/10358317/201671961-6eb2815e-67e1-43d5-9e36-49bcebe0dad4.png)


- Create 'site.yml' file
- To prioritize the tasks, for updating, 'pre_tasks' is used
- For different hosts, 3 different host groups are created: all, web_servers and database_servers
```
---

- hosts: all
  become: true
  pre_tasks:

  - name: install updates (CentOS)
    dnf:
      update_only: yes
      update_cache: yes
    when: ansible_distribution == "CentOS"

  - name: install updates (Ubuntu)
    apt:
      upgrade: dist
      update_cache: yes
    when: ansible_distribution == "Ubuntu"

- hosts: web_servers
  become: true
  tasks:

  - name: install apache and php (CentOS)
    dnf:
      name:
        - httpd
        - php
      state: latest
    when: ansible_distribution == "CentOS"

  - name: install apache and php (Ubuntu)
    apt:
      name:
        - apache2
        - libapache2-mod-php
      state: latest
    when: ansible_distribution == "Ubuntu"
    
- hosts: database_servers
  become: true
  tasks:

  - name: install MariaDB (CentOS)
    dnf:
      name: mariadb
      state: latest
    when: ansible_distribution == "CentOS"

  - name: install MariaDB (Ubuntu)
    apt:
      name: mariadb-server
      state: latest
    when: ansible_distribution == "Ubuntu"
```    

- Run 

```
ansible-playbook --ask-become-pass site.yml
```

![image](https://user-images.githubusercontent.com/10358317/201669957-5141bd02-b42a-4ceb-8750-d2aee3fb9716.png)

- When we control whether mariadb is installed or not on database_servers (node2), it can be seen that it was installed on node2.

```
multipass shell node2
systemctl status mariadb
```

![image](https://user-images.githubusercontent.com/10358317/201671418-42d8734a-d55f-4247-8451-db45e7a889a4.png)

