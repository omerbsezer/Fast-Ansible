## LAB: Refactoring / Improving Playbook

This scenario shows:
- how to refactor / improve playbook 

### Prerequisite

- You should have a look following lab, nodes that are created in that LAB, are using in ansible commands
  - [LAB: Multipass-SSH Configuration (Create Ansible Test Environment)](https://github.com/omerbsezer/Fast-Ansible/blob/main/Multipass-SSH-Configuration.md)

### Steps

- We have playbook that is created in this LAB: https://github.com/omerbsezer/Fast-Ansible/blob/main/Implement-First-Playbook.md

``` 
---

- hosts: all
  become: true
  tasks:

  - name: update repository index
    apt:
      update_cache: yes
    when: ansible_distribution in ["Debian", "Ubuntu"]

  - name: install apache2 package
    apt:
      name: apache2
      state: latest
    when: ansible_distribution == "Ubuntu"

  - name: add php support for apache
    apt:
      name: libapache2-mod-php
      state: latest
    when: ansible_distribution == "Ubuntu"

  - name: update repository index
    dnf:
      update_cache: yes
    when: ansible_distribution == "CentOS"

  - name: install apache2 package
    dnf:
      name: httpd
      state: latest
    when: ansible_distribution == "CentOS"

  - name: add php support for apache
    dnf:
      name: php
      state: latest
    when: ansible_distribution == "CentOS"
``` 

#### Consolidate same modules tasks into one task

- Consolidate 'apt' and 'dnf' modules into one group both for Ubuntu and CentOS. 
- For both of them, they install two packages and update repository index.
```
---

- hosts: all
  become: true
  tasks:

  - name: update repository index, install apache2 and php package for Ubuntu
    apt:
      name:
        - apache2
        - libapache2-mod-php
      state: latest
      update_cache: yes
    when: ansible_distribution == "Ubuntu"

  - name: update repository index, install apache2 and php package for CentOS
    dnf:
      name:
        - httpd
        - php
      state: latest
      update_cache: yes
    when: ansible_distribution == "CentOS"
```

#### Consolidate into one task with Variables

- Update install_apache.yml file and add variables: apache_package, php_package
- Change 'apt' and 'dnf' to 'package'
 
```
---

- hosts: all
  become: true
  tasks:

  - name: update repository index, install apache and php packages
    package:
      name:
        - "{{ apache_package }}"
        - "{{ php_package }}"
      state: latest
      update_cache: yes
```

- Update inventory and add variables

```
172.21.79.85  apache_package=apache2 php_package=libapache2-mod-php
172.21.76.101 apache_package=apache2 php_package=libapache2-mod-php
172.21.76.102 apache_package=httpd php_package=php
```

- Run: 

```
ansible-playbook --ask-become-pass install_apache.yml
```

![image](https://user-images.githubusercontent.com/10358317/201663202-00e9288e-9c9f-4c2b-95f8-d7146a590da7.png)

