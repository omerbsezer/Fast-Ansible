## LAB: Roles

This scenario shows:
- how to create roles 

### Prerequisite

- You should have a look following lab, nodes that are created in that LAB, are using in ansible commands
  - [LAB: Multipass-SSH Configuration (Create Ansible Test Environment)](https://github.com/omerbsezer/Fast-Ansible/blob/main/Multipass-SSH-Configuration.md)

### Steps

- Update site.yml with 'roles'.

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

- hosts: all
  become: true
  roles:
    - base

- hosts: web_servers
  become: true
  roles:
    - web_servers

- hosts: database_servers
  become: true
  roles:
    - database_servers
```

![image](https://user-images.githubusercontent.com/10358317/202458768-ffbc6907-4659-4a43-8629-454f0b4a9a7f.png)

- Create directories.

```
mkdir role
cd role
mkdir base
mkdir web_servers
mkdir database_servers
```

![image](https://user-images.githubusercontent.com/10358317/202450210-f6f6f3c4-7a50-4680-bce9-13cad264655d.png)

- Create subdirectories

```
cd role
mkdir base/tasks
mkdir web_servers/tasks
mkdir database_servers/tasks
```

![image](https://user-images.githubusercontent.com/10358317/202452013-eaa1b281-cc1f-4259-b29d-73bf98ba4dad.png)

- Create file 'main.yml' under 'role/base/tasks' with 'nano main.yml' and copy followings (taken from previous 'site.yml'):

```
- name: add ssh key for new user
  authorized_key:
    user: newuser111
    key: "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCdEJ8xu1C0kJ4Y39x2bfptMQVdxnPDXrkjmDvB44oDV78yKWg/0B/kacMqiEaaiEyAedHlk>
```

- Create file 'main.yml' under 'role/database_servers/tasks' with 'nano main.yml' and copy followings (taken from previous 'site.yml'):

```
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

- Create file 'main.yml' under 'role/web_servers/tasks' with 'nano main.yml' and copy followings (taken from previous 'site.yml'):

```
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
```

- Create directory 'roles/web_servers/files' and files in the 'files' directory

```
mkdir files
cp ../../files/default_site.html files/default_site.html
```

![image](https://user-images.githubusercontent.com/10358317/202455685-4becaa8e-6164-42bb-80f7-18471b21478d.png)

- Printscreen shows the structure of the directories. Structure should be like that, because Ansible finds the tasks with that way (e.g. roles/[nameofrole]/tasks/main.yml)

![image](https://user-images.githubusercontent.com/10358317/202458020-39562296-fb49-4fec-94cc-e071e29a2c9a.png)

- Run ansible-playbook

```
ansible-playbook site.yml
```

![image](https://user-images.githubusercontent.com/10358317/202458341-cd28db4c-4ba2-4b0b-8edd-0b245d4f6f07.png)

