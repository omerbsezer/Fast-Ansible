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

- If you enter "never-existed-tect-package" as apt name, you can see that it cannot install on the nodes, because it does not exist.

![image](https://user-images.githubusercontent.com/10358317/201104092-7a38235c-1a48-4f16-8c2e-9269be6d7faa.png)

![image](https://user-images.githubusercontent.com/10358317/201103961-7a10a711-d6e4-4aac-b05c-f5d5172f25ad.png)

- Add "apt update" and install other packages

``` 
---

- hosts: all
  become: true
  tasks:

  - name: update repository index
    apt:
      update_cache: yes

  - name: install apache2 package
    apt:
      name: apache2

  - name: add php support for apache
    apt:
      name: libapache2-mod-php
``` 

![image](https://user-images.githubusercontent.com/10358317/201105158-4b3e598b-0726-444f-8844-ee99fc8f8d82.png)

``` 
ansible-playbook --ask-become-pass install_apache.yml
``` 

![image](https://user-images.githubusercontent.com/10358317/201105473-2697a57a-4334-484f-97d9-501452007259.png)

- Add "state: latest" to install latest version of the app

``` 
---

- hosts: all
  become: true
  tasks:

  - name: update repository index
    apt:
      update_cache: yes

  - name: install apache2 package
    apt:
      name: apache2
      state: latest

  - name: add php support for apache
    apt:
      name: libapache2-mod-php
      state: latest
```

![image](https://user-images.githubusercontent.com/10358317/201106278-537221bf-878a-4ee3-b211-83e9e52e252f.png)





