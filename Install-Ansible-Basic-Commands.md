## LAB: Install Ansible and Test Basic Ansible (Ad-Hoc) Commands

This scenario shows:
- how to install ansible
- how to use basic commands

### Prerequisite

- You should have a look following lab, nodes that are created in that LAB, are using in ansible commands
  - [LAB: Multipass-SSH Configuration (Create Ansible Test Environment)](https://github.com/omerbsezer/Fast-Ansible/blob/main/Multipass-SSH-Configuration.md)

### Steps

- Install Ansible on all nodes

``` 
sudo apt install ansible
``` 

- Create directory and inventory file to put managed nodes' IPs

![image](https://user-images.githubusercontent.com/10358317/201087999-7bbb7f0b-acdf-475a-b8c8-cf0f689bc29b.png)

- Put the managed nodes' IPs into the inventory file

![image](https://user-images.githubusercontent.com/10358317/201088310-e7859682-dc0d-46f5-bac4-ba553e38be90.png)

- Ping all nodes with ansible

``` 
ansible all -i inventory -m ping
``` 

![image](https://user-images.githubusercontent.com/10358317/201089266-84c032d5-7647-45ec-b44a-0323cf7f6274.png)

**NOTE:** If you use secure private SSH keys on each nodes (copied these keys on each nodes), it should be used "ansible all --key-file ~/.ssh -i inventory -m ping" 

- Create config file (ansible.cfg)

``` 
touch ansible.cfg
# copy followings:
[defaults]
inventory = inventory
# private_key_file = ~/.ssh/ansible
``` 

![image](https://user-images.githubusercontent.com/10358317/201090216-084d1328-88fc-462f-b307-d95c8d8b752d.png)

![image](https://user-images.githubusercontent.com/10358317/201090391-67057ecd-68a9-4aa6-af33-af5fcd099840.png)

- Using config file (ansible.cfg), we can use short commands

![image](https://user-images.githubusercontent.com/10358317/201090690-752feb31-9b42-42df-a89f-63e3092b4a32.png)

- List all hosts

```
ansible all --list-hosts
``` 
![image](https://user-images.githubusercontent.com/10358317/201090920-d5d2a294-698a-4e62-89e7-7df3f1d1834d.png)

- Gather all nodes' information (all resources' information: cpu, ip, ssd, etc.) from all hosts

```
# -m parameter means ansible module
ansible all -m gather_facts
``` 

![image](https://user-images.githubusercontent.com/10358317/201091229-60ab2618-ba53-4460-96f8-7c69a4a9c6b1.png)

- Gather information from specific node 

```
ansible all -m gather_facts --limit 172.26.215.23
```

- Run "sudo apt update" for all nodes using ansible
- As it is seen in the printscreen, it does not work. 

```
ansible all -m apt -a update_cache=true
```

![image](https://user-images.githubusercontent.com/10358317/201094159-89918be8-1d73-4a10-b346-4d54a1bc104f.png)

- The reason why "sudo apt update" does not work is to enter "sudo" password for all nodes.
- For now, we are assigning same password for all nodes (node1, node2). Later, it will be shown for different passwords.

```
sudo passwd ubuntu
```

![image](https://user-images.githubusercontent.com/10358317/201094654-23381802-43a2-4261-892b-900244019bcc.png)
![image](https://user-images.githubusercontent.com/10358317/201094744-d8edfd82-9c5a-4bb8-9fc5-7e9f5f4567c1.png)
![image](https://user-images.githubusercontent.com/10358317/201094827-5ddd50dd-bd26-47b9-b266-5d997678774b.png)

- Run "sudo apt update" for all nodes using "BECOME PASS", enter the common password when it asks

```
ansible all -m apt -a update_cache=true --become --ask-become-pass
```

![image](https://user-images.githubusercontent.com/10358317/201095106-cfa74f25-9ae6-4ca2-b34a-061ed5d6622d.png)

- Install specific package "sudo apt get snapd"

```
ansible all -m apt -a name=snapd --become --ask-become-pass
# latest version
ansible all -m apt -a "name=snapd state=latest" --become --ask-become-pass
```

![image](https://user-images.githubusercontent.com/10358317/201097511-9b0893f5-120c-4af1-be6d-a35fc15681a5.png)



