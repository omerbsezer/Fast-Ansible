## LAB: Host Variables

This scenario shows:
- how to create host variables

### Prerequisite

- You should have a look following lab, nodes that are created in that LAB, are using in ansible commands
  - [LAB: Multipass-SSH Configuration (Create Ansible Test Environment)](https://github.com/omerbsezer/Fast-Ansible/blob/main/Multipass-SSH-Configuration.md)

### Steps

- Create 'host_vars' directory

```
mkdir host_vars
```

![image](https://user-images.githubusercontent.com/10358317/202507475-c42f278e-0999-4cdd-b9dd-c60d52d44639.png)

- If the nodes in the inventory are defined with IPs, create file with 'IP.yml' (if they are defined with domain name, files are 
'domain_name.yml')

- Add following variable into the file:

```
apache_package_name: apache2
apache_service: apache2
php_package_name: libapache2-mod-php
```

![image](https://user-images.githubusercontent.com/10358317/202508266-b824fb3b-c5bf-4b91-9b20-a8a5fee4181b.png)

- We have host.yml files and host_vars in the yml file

![image](https://user-images.githubusercontent.com/10358317/202508480-f7e094da-0d31-4327-a219-82e690b41acf.png)

- Update 'roles/web_servers/tasks/main.yml' by adding variables (with quotation mark ") that are defined in files in host_vars directory.
- For each node, defined host variables are used for variables. 

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
  register: apache2_service

- name: restart apache (Ubuntu)
  tags: ubuntu,apache,apache2
  service:
    name: "{{ apache_service }}"
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

![image](https://user-images.githubusercontent.com/10358317/202510904-92f51a73-83d1-4c19-b376-37d8c236e7c4.png)

Run:

```
ansible-playbook site.yml
```

![image](https://user-images.githubusercontent.com/10358317/202511367-dd789f83-a7d5-40e1-9f2c-e3fec2e1f44c.png)

