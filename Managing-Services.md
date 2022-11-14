## LAB: Managing Services

This scenario shows:
- how to manage services

### Prerequisite

- You should have a look following lab, nodes that are created in that LAB, are using in ansible commands
  - [LAB: Multipass-SSH Configuration (Create Ansible Test Environment)](https://github.com/omerbsezer/Fast-Ansible/blob/main/Multipass-SSH-Configuration.md)

### Steps

#### Service Start

- Update site.yml file, Add followings:
```
  - name: start apache (Ubuntu)
    tags: ubuntu,apache,apache2
    service:
      name: apache2
      state: started
      enabled: yes
    when: ansible_distribution == "Ubuntu"
```
- Updated site.yml 

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
    
  - name: start apache (Ubuntu)
    tags: ubuntu,apache,apache2
    service:
      name: apache2
      state: started
      enabled: yes
    when: ansible_distribution == "Ubuntu"    
    
  - name: copy default (index) html file for site
    tags: apache,apache2,httpd
    copy:
      src: default_site.html
      dest: /var/www/html/index.html
      owner: root
      group: root
      mode: 0644    
      
  - name: install unzip
    package:
      name: unzip

  - name: install terraform
    unarchive:
      src: https://releases.hashicorp.com/terraform/1.3.4/terraform_1.3.4_linux_amd64.zip
      dest: /usr/local/bin
      remote_src: yes
      owner: root
      group: root
      mode: 0755  
      
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
- Stop apache2 service and see the service is stopped.

``` 
systemctl stop apache2
systemctl status apache2
``` 

![image](https://user-images.githubusercontent.com/10358317/201748361-1423d4e9-f880-44f6-9f3c-c7c3c0afe712.png)

- Run 

``` 
ansible-playbook --ask-become-pass site.yml
``` 

- It is seen that apache2 service is started:

![image](https://user-images.githubusercontent.com/10358317/201749062-dcf7cd41-4133-4cc9-8f6b-a3e0b35a4cda.png)

#### Change Service Config File

- Go to node1 and run:

```
sudo nano /etc/apache2/sites-available/000-default.conf
```

![image](https://user-images.githubusercontent.com/10358317/201751273-ed32c787-f360-48a2-aa66-2fedfd4e9391.png)

- Add following tasks into the webserver side of site.yml.

```
  - name: change email address for admin (Ubuntu)
    tags: ubuntu,apache,apache2
    lineinfile:
      path: /etc/apache2/sites-available/000-default.conf
      regexp: '^ServerAdmin'
      line: ServerAdmin somebody@somewhere.com
    when: ansible_distribution == "Ubuntu"
    register: apache2_service

  - name: restart apache (Ubuntu)
    tags: ubuntu,apache,apache2
    service:
      name: apache2
      state: restarted
    when: apache2_service.changed
```

- As can seen printscreen, when the keyword is written in the register part (apache2_service), is changed, restart service is restarted. The task depends on the registered variable status. 

![image](https://user-images.githubusercontent.com/10358317/201753745-29e9e879-770a-46e5-a315-1da40f0cd1fa.png)

- **IMPORTANT:** If the register variable is defined consecutively, if second task does not run with same register variable (because first same task handled it), because second one does not change, so restart task does not run.   

![image](https://user-images.githubusercontent.com/10358317/201755326-44284c95-db0d-46c3-8f22-f224d845bd5b.png)

- Updated site.yml:

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
    
  - name: start apache (Ubuntu)
    tags: ubuntu,apache,apache2
    service:
      name: apache2
      state: started
      enabled: yes
    when: ansible_distribution == "Ubuntu"
    
  - name: change email address for admin (Ubuntu)
    tags: ubuntu,apache,apache2
    lineinfile:
      path: /etc/apache2/sites-available/000-default.conf
      regexp: 'ServerAdmin webmaster@localhost'
      line: '       ServerAdmin somebody@somewhere.com'
    when: ansible_distribution == "Ubuntu"
    register: apache2_service

  - name: restart apache (Ubuntu)
    tags: ubuntu,apache,apache2
    service:
      name: apache2
      state: restarted
    when: apache2_service.changed
    
  - name: copy default (index) html file for site
    tags: apache,apache2,httpd
    copy:
      src: default_site.html
      dest: /var/www/html/index.html
      owner: root
      group: root
      mode: 0644    
      
  - name: install unzip
    package:
      name: unzip

  - name: install terraform
    unarchive:
      src: https://releases.hashicorp.com/terraform/1.3.4/terraform_1.3.4_linux_amd64.zip
      dest: /usr/local/bin
      remote_src: yes
      owner: root
      group: root
      mode: 0755  
      
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

- It can be seen that it is changed:

![image](https://user-images.githubusercontent.com/10358317/201753411-ed5e38cd-1877-4261-b63f-72e413cd4a7c.png)

## Reference

- https://www.youtube.com/watch?v=soeBHGAMkoQ&list=PLT98CRl2KxKEUHie1m24-wkyHpEsa4Y70&index=12
