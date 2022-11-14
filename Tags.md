## LAB: Adding Tags

Tags are useful when we want to run specific part of the playbook. If we tagged with specific keywords, only tagged part would run.  

This scenario shows:
- how to add tags and run according to specific tags

### Prerequisite

- You should have a look following lab, nodes that are created in that LAB, are using in ansible commands
  - [LAB: Multipass-SSH Configuration (Create Ansible Test Environment)](https://github.com/omerbsezer/Fast-Ansible/blob/main/Multipass-SSH-Configuration.md)

### Steps

- Tags are added with "tags" keyword. "tags: Always" run always, other tags could be custom defined (e.g. apache, mariadb, etc.)

``` 
---

- hosts: all
  become: true
  pre_tasks:

  - name: install updates (CentOS)
    tags: always
    dnf:
      update_only: yes
      update_cache: yes
    when: ansible_distribution == "CentOS"

  - name: install updates (Ubuntu)
    tags: always
    apt:
      upgrade: dist
      update_cache: yes
    when: ansible_distribution == "Ubuntu"

- hosts: web_servers
  become: true
  tasks:

  - name: install apache and php (CentOS)
    tags: centos,apache,httpd
    dnf:
      name:
        - httpd
        - php
      state: latest
    when: ansible_distribution == "CentOS"

  - name: install apache and php (Ubuntu)
    tags: ubuntu,apache,apache2
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
    tags: centos,db,mariadb
    dnf:
      name: mariadb
      state: latest
    when: ansible_distribution == "CentOS"

  - name: install MariaDB (Ubuntu)
    tags: ubuntu,db,mariadb-server
    apt:
      name: mariadb-server
      state: latest
    when: ansible_distribution == "Ubuntu"
``` 

- To see the defined tags, run following:

```
ansible-playbook --list-tags site.yml
```

![image](https://user-images.githubusercontent.com/10358317/201675534-6773039e-b45e-4f1f-b473-3a1ab1059f69.png)

- When we want to run only tasks which contains 'Ubuntu' tag, run following:

```
ansible-playbook --tags ubuntu --ask-become-pass site.yml
```

- It can be seen below, only 'ubuntu' tagged tasks and 'always' tagged tasks were run. "Gathering Facts" always run to collect server data.

![image](https://user-images.githubusercontent.com/10358317/201676220-0cb5dfc4-3a29-4de0-b9eb-96a3fcc275dd.png)

- When we want to run only tasks which contains 'db' tag, run following:

```
ansible-playbook --tags db --ask-become-pass site.yml
```

- It can be seen below, only 'db' tagged tasks and 'always' tagged tasks were run.

![image](https://user-images.githubusercontent.com/10358317/201676636-7043e4e5-2277-4273-8274-e934e5ad1bb8.png)

- When we want to run only tasks which contains 'always' tag, run following:

```
ansible-playbook --tags always --ask-become-pass site.yml
```

![image](https://user-images.githubusercontent.com/10358317/201677188-2df9f569-89ea-4691-952a-db812e47ffe3.png)
