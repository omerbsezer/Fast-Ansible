## LAB: Templates

This scenario shows:
- how to create templates.

### Prerequisite

- You should have a look following lab, nodes that are created in that LAB, are using in ansible commands
  - [LAB: Multipass-SSH Configuration (Create Ansible Test Environment)](https://github.com/omerbsezer/Fast-Ansible/blob/main/Multipass-SSH-Configuration.md)

### Steps

- Create 'templates' directory under the '/roles/base/'
- Copy 'sshd_config' file under 'templates' and create jinja2 file (.j2)

```
mkdir templates
cd templates
cp /etc/ssh/sshd_config sshd_config_ubuntu.j2
 ```

![image](https://user-images.githubusercontent.com/10358317/202679439-45eceefc-0b29-418c-8b75-5558e722fd87.png)


- Open 'sshd_config_ubuntu.j2' file and add 'AllowUsers'.

```
nano sshd_config_ubuntu.j2
# add following
AllowUsers {{ ssh_users }}
```

![image](https://user-images.githubusercontent.com/10358317/202685736-463d1e46-2f00-4852-a8c3-f6f62e861399.png)

- Go to 'host_vars' directory,
- Change the content of this host_vars files

```
nano 172.21.69.156.yml
# add following
ssh_users: "newuser2022"
ssh_template_file: sshd_config_ubuntu.j2
```

![image](https://user-images.githubusercontent.com/10358317/202680949-69c1f1dd-b51f-4900-8bdd-992955707293.png)

- File contents:

```
apache_package_name: apache2
apache_service: apache2
php_package_name: libapache2-mod-php
ssh_users: "newuser2022"
ssh_template_file: sshd_config_ubuntu.j2
```

![image](https://user-images.githubusercontent.com/10358317/202682564-b6ed46b2-b155-4366-9acc-6158236e5643.png)

- Add another task to use templates

![image](https://user-images.githubusercontent.com/10358317/202683263-66005498-8f3a-43ac-9745-ec63f6788ddc.png)

- Open and add ('nano main.yml')
- After generating sshh_config file, it triggers handler (restart_ssh)

```
- name: generate sshd_config file using templates
  tags: ssh
  template:
    src: "{{ ssh_template_file }}"
    dest: /etc/ssh/sshd_config
    owner: root
    group: root
    mode: 0644
  notify: restart_sshd
```  

![image](https://user-images.githubusercontent.com/10358317/202692366-c606995e-0eb5-4f7b-82ec-7a398750df1b.png)

- Create handlers and create 'main.yml' file

```
- name: restart_sshd
  service:
    name: sshd
    state: restarted
```

![image](https://user-images.githubusercontent.com/10358317/202687675-fcc12387-9f4f-4f70-a357-797bf3ad1738.png)

- Run:

```
ansible-playbook site.yml
```
![image](https://user-images.githubusercontent.com/10358317/202692817-02c63dd3-86e4-47da-bec9-864cbc3a3510.png)

- After running playbook, when we go to the 'node1' to see the content of sshd_config ('nano /etc/ssh/sshd_config').
- 'AllowUsers newuser2022' is added.

![image](https://user-images.githubusercontent.com/10358317/202694177-b202a08c-ff59-4739-b0e9-922c7e5d8d41.png)

## Reference

- https://www.youtube.com/watch?v=s8F_YWGHeDM&list=PLT98CRl2KxKEUHie1m24-wkyHpEsa4Y70&index=17

