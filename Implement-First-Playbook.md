## LAB: Implement First Playbook

This scenario shows:
- how to create ansible playbooks

### Prerequisite

- You should have a look following lab, nodes that are created in that LAB, are using in ansible commands
  - [LAB: Multipass-SSH Configuration (Create Ansible Test Environment)](https://github.com/omerbsezer/Fast-Ansible/blob/main/Multipass-SSH-Configuration.md)

### Steps

- Create playbook to install apache2 app on nodes

``` 
nano install_apache.yml
# copy followings:
---

- hosts: all
  become: true
  tasks:

  - name: install apache2 package
    apt:
      name: apache2
``` 

![image](https://user-images.githubusercontent.com/10358317/201101204-780321bf-57b7-450b-89e9-1c5d6deb5e39.png)

![image](https://user-images.githubusercontent.com/10358317/201100926-480de972-078e-4541-9f06-30b625c71585.png)

- Run playbook

``` 
ansible-playbook --ask-become-pass install_apache.yml
``` 

- Under Task "install apache2 package", status are changed, this means installing apache2 packet on the nodes successfully

![image](https://user-images.githubusercontent.com/10358317/201101843-efc4262d-5506-404e-b505-0d91131154df.png)

- If you run again that command, it is seen that "ok" under the Task "install apache2 package", it means that there is no change

![image](https://user-images.githubusercontent.com/10358317/201102379-69fe5e2b-7793-421d-add0-25d88b19b969.png)

- When you enter the one of the nodes' IP on the browser, you can see that the apache2 server is installed on it

![image](https://user-images.githubusercontent.com/10358317/201103096-a62b7d08-1208-485f-8bd4-de5b5c7b1e06.png)


