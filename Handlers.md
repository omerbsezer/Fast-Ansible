## LAB: Handlers

This scenario shows:
- how to create handlers.

### Prerequisite

- You should have a look following lab, nodes that are created in that LAB, are using in ansible commands
  - [LAB: Multipass-SSH Configuration (Create Ansible Test Environment)](https://github.com/omerbsezer/Fast-Ansible/blob/main/Multipass-SSH-Configuration.md)

### Steps

- Open 'roles/web_servers/tasks/main.yml', update followings (add notify: restart_apache):
- The variables defined near notify calls the task that is defined in 'handlers/main.yml'

```
- name: change email address for admin (Ubuntu)
  tags: ubuntu,apache,apache2
  lineinfile:
    path: /etc/apache2/sites-available/000-default.conf
    regexp: 'ServerAdmin webmaster@localhost'
    line: '       ServerAdmin somebody@somewhere.com'
  when: ansible_distribution == "Ubuntu"
  notify: restart_apache
 ```
 
 ![image](https://user-images.githubusercontent.com/10358317/202517097-3f895a18-af2f-4103-a5af-18632dea71df.png)

  
- All 'roles/web_servers/tasks/main.yml':
```
- name: install apache and php
  tags: ubuntu,apache,apache2
  apt:
    name:
      - "{{ apache_package_name }}"
      - "{{ php_package_name }}"
    state: latest

- name: start apache
  tags: ubuntu,apache,apache2
  service:
    name: "{{ apache_service }}"
    state: started
    enabled: yes

- name: change email address for admin (Ubuntu)
  tags: ubuntu,apache,apache2
  lineinfile:
    path: /etc/apache2/sites-available/000-default.conf
    regexp: 'ServerAdmin webmaster@localhost'
    line: '       ServerAdmin somebody@somewhere.com'
  when: ansible_distribution == "Ubuntu"
  notify: restart_apache

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

- Create 'handlers' directory under 'roles/web_servers' and create 'main.yml' 

```
- name: restart_apache
  service:
    name: "{{ apache_service }}"
    state: restarted
```

![image](https://user-images.githubusercontent.com/10358317/202516238-4bd6f722-656f-4bd7-a3a0-135afc562a63.png)

- Run:

```
ansible-playbook site.yml
```

- Update the mail address to trigger the handler (restart_apache). Unless the task that has notify is changed, it cannot trigger handler task.   

![image](https://user-images.githubusercontent.com/10358317/202518007-922c1f40-0c66-4a89-a121-8b1c16d5d30b.png)

- Handler is called.

![image](https://user-images.githubusercontent.com/10358317/202517584-71df6b36-314b-4ceb-af23-6d73cded4e5b.png)

- File/Directory structure:

![image](https://user-images.githubusercontent.com/10358317/202518840-c9aa8a6c-a134-444d-9e00-53d60b1ad7dc.png)


