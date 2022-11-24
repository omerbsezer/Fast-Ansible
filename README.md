# Fast-Ansible
This repo covers Ansible with HowTo: Hands-on LABs (using Multipass: Ubuntu Lightweight VMs): Ad-Hoc Commands, Modules, Playbooks, Tags, Managing Files and Servers, Users, Roles, Handlers, Host Variables, Templates and many details. Possible usage scenarios are aimed to update over time.

**Keywords:** Ansible, Multi-PC Configuration.

# Quick Look (HowTo): Scenarios - Hands-on LABs
- [LAB: Multipass-SSH Configuration (Create Ansible Test Environment)](https://github.com/omerbsezer/Fast-Ansible/blob/main/Multipass-SSH-Configuration.md)
- [LAB: Install Ansible and Test Basic Ansible (Ad-Hoc) Commands](https://github.com/omerbsezer/Fast-Ansible/blob/main/Install-Ansible-Basic-Commands.md)
- [LAB: Implement First Playbook](https://github.com/omerbsezer/Fast-Ansible/blob/main/Implement-First-Playbook.md)
- [LAB: Playing Docker Module](https://github.com/omerbsezer/Fast-Ansible/blob/main/Docker-Module.md)
- [LAB: Important (Mostly Possible Used) Modules Sample Tasks](https://github.com/omerbsezer/Fast-Ansible/blob/main/Important-Modules-Sample-Tasks.md)
- [LAB: Refactoring / Improving Playbook](https://github.com/omerbsezer/Fast-Ansible/blob/main/Refactoring-Playbook.md)
- [LAB: Targeting Specific Nodes (Grouping)](https://github.com/omerbsezer/Fast-Ansible/blob/main/Targeting-Specific-Node.md)
- [LAB: Adding Tags](https://github.com/omerbsezer/Fast-Ansible/blob/main/Tags.md)
- [LAB: Managing Files](https://github.com/omerbsezer/Fast-Ansible/blob/main/Managing-Files.md)
- [LAB: Managing Services](https://github.com/omerbsezer/Fast-Ansible/blob/main/Managing-Services.md)
- [LAB: Adding Users](https://github.com/omerbsezer/Fast-Ansible/blob/main/Adding-User.md)
- [LAB: Roles](https://github.com/omerbsezer/Fast-Ansible/blob/main/Roles.md)
- [LAB: Host Variables](https://github.com/omerbsezer/Fast-Ansible/blob/main/Host-Variables.md)
- [LAB: Handlers](https://github.com/omerbsezer/Fast-Ansible/blob/main/Handlers.md)
- [LAB: Templates](https://github.com/omerbsezer/Fast-Ansible/blob/main/Templates.md)

# Table of Contents
- [Motivation](#motivation)
- [What is Ansible?](#whatIsAnsible)
- [How Ansible Works?](#howAnsibleWorks)
- [Creating LAB Environment](#labEnvironment)
- [Ansible Basic (Ad-Hoc) Commands](#commands)    
- [Ansible Modules](#modules)
- [Ansible Playbooks](#playbooks)
- [Inventory File - Targeting Specific Nodes](#inventory)
- [Tags](#tags)
- [Managing Files](#files)
- [Managing Services](#services)
- [Adding Users](#users)
- [Roles](#roles)
- [Host Variables](#hostvariables)
- [Handlers](#handlers)
- [Templates](#templates)
- [Debugging](#debugging)
- [Details](#details)
- [Other Useful Resources Related Ansible](#resource)
- [References](#references)

## Motivation <a name="motivation"></a>

Why should we use / learn Ansible? 

- Ansible automates tasks and commands to manage multiple nodes (servers, PCs).
- Ansible is a state-of-the-art automation tool. Many companies use it.
- Ansible can be used both on-premises and cloud environment.
- It is free, open source (https://github.com/ansible/ansible) and has huge community.
- Commands, tasks, codes turn into the Infrastructure As Code (IaC).
  - With IaC, tasks is savable, versionable, repetable and testable.
  - With IaC, desired configuration is defined as 'Declerative Way'.
- **Agentless:** On the worker node, any agent app is not required to run.
- **Parallel Run:** Ansible runs the same task in multiple hosts by default in parallel.
- It runs tasks both on Linux and Windows PCs.
- It has well-designed documentation (https://docs.ansible.com)
- Ansible uses SSH to communicate with other nodes.
- Ansible handles many tasks using its modules.

  ![image](https://user-images.githubusercontent.com/10358317/202701707-b160e35c-7a05-43e8-93c7-b626c8054aa9.png) (ref: medium)

- Ansible also is used by other important applications (e.g. Open Source Gating CI Tools: [Zuul-CI](https://zuul-ci.org/))

- Ansible is used for configuration management on the nodes, Terraform is provisioning tool that uses to create/provision Cloud Infrastructure objects/items (e.g. Virtual Private Cloud, Virtual Machines, Subnets, etc.)

  ![image](https://user-images.githubusercontent.com/10358317/202700302-d651cf08-dd55-44ea-a88c-8ee4186d9438.png) (ref: ibm.github.io)

- Ansible can be easily integrated with different technologies (e.g. [Terraform](https://www.terraform.io/)).

## What is Ansible? <a name="whatIsAnsible"></a>
- "Ansible is a software tool that provides simple but powerful automation for cross-platform computer support." (ref: Opensource.com)

## How Ansible Works? <a name="howAnsibleWorks"></a>

- In Ansible, there are two categories of computers: the control node and managed nodes. The control node is a computer that runs Ansible. There must be at least one control node, although a backup control node may also exist. A managed node is any device being managed by the control node. 
- Ansible works by connecting to nodes (clients, servers, or whatever you're configuring) on a network, and then sending a small program called an Ansible module to that node. Ansible executes these modules over SSH and removes them when finished.(ref: Opensource.com)
- The only requirement for this interaction is that your Ansible control node has login access to the managed nodes. SSH keys are the most common way to provide access, but other forms of authentication are also supported. (ref: Opensource.com)
- There are files that are for configuration and usage of Ansible:
  - **Inventory File**: It contains and groups worker nodes' IP and domain names. Ansible knows and sends commands using these file, typically located at /etc/ansible/hosts.
  - **Configuration (.cfg) File**: It contains configuration (e.g. inventory file, key_file, remote_user, etc.)
  - **Playbook**: Playbooks are one of the core features of Ansible and tell Ansible what to execute. It is a file containing a series of tasks to be executed on a remote server. Playbooks are written in YAML format. 
- Other Important Parts:  
  - **Control Machine / Node**: a system where Ansible is installed and configured to connect and execute commands on nodes.
  - **Node (Worker node)**: a server controlled by Ansible.
  - **Role**: a collection of playbooks and other files that are relevant to a goal such as installing a web server.
  - **Play**: a full Ansible run. A play can have several playbooks and roles, included from a single playbook that acts as entry point.

  ![image](https://user-images.githubusercontent.com/10358317/202699093-62fcc145-c023-43ed-af51-0866393f0701.png) (ref: kreyman.de)

## Creating LAB Environment <a name="labEnvironment"></a>

- For testing Ansible, Ansible modules, it is required to get multiple PC or virtual machine. 
- So we are using Multipass that is lightweight, fast, easy to use Ubuntu VM (on demand for any workstation).
- Install on Linux, Windows and MacOs: https://multipass.run/install
- [LAB: Multipass-SSH Configuration (Create Ansible Test Environment)](https://github.com/omerbsezer/Fast-Ansible/blob/main/Multipass-SSH-Configuration.md)

## Ansible Basic (Ad-Hoc) Commands <a name="commands"></a>

- Commands can be sent to the all worker nodes from control node.  
- Code structure: 
  ![image](https://user-images.githubusercontent.com/10358317/203532819-acf44653-de14-426b-9656-35fa82bd4721.png) 

- Sample Commands:
``` 
# collect information from specific IP
ansible all -m gather_facts --limit 172.26.215.23
# collect information from all hosts
ansible all -m gather_facts
# update all nodes (sudo apt-get update), 'become' for sudo (give all nodes same password with 'passwd')
ansible all -m apt -a update_cache=true --become --ask-become-pass
# install snapd for all ubuntu nodes (sudo apt-get install snapd)
ansible all -m apt -a "name=snapd state=latest" --become --ask-become-pass
# upgrade all nodes (sudo apt-get upgrade)
ansible all -m apt -a upgrade=dist --become --ask-become-pass
# run shell commands
ansible all -m shell -a "cat /proc/meminfo|head -2" 
ansible web_servers -m shell -a "cat /proc/meminfo|head -2" 
# to learn disk space
ansible all -m command -a "df -h"
ansible all  -a "df -h"
# check the status of the httpd service
ansible all -m service -a "name=httpd"
# get the uptime with shell module
ansible all -m shell -a uptime
# create testfile under '/tmp' with mode=0755
ansible all -m file -a "path=/tmp/testfile state=touch mode=0755"
# list all hosts
ansible all --list-hosts
``` 

- If you define your inventory file (nano inventory) and group the servers with keywords, you can use 'web_servers' and 'database_servers' in the commands (above).
``` 
[web_servers]
172.21.67.249

[database_servers]
172.21.75.98
``` 
- Go to LAB to learn:
  - [LAB: Install Ansible and Test Basic Ansible (Ad-Hoc) Commands](https://github.com/omerbsezer/Fast-Ansible/blob/main/Install-Ansible-Basic-Commands.md)

## Ansible Modules <a name="modules"></a>

- Modules: Small programs that do actual job (one small specific task).
  - Sample modules (some of them):
    - Install app packages (e.g. sudo apt install),
    - Upgrade, update repository index (e.g. sudo apt upgrade, sudo apt update),
    - Create and copy files, Start Services,
    - Download Docker Image, Start/Stop Docker containers,
    - Start/Stop Nginx Server, Create Cloud instance, etc.     
- Control node sends these modules to the nodes.

- Go to LAB to learn:
  - [LAB: Playing Docker Module](https://github.com/omerbsezer/Fast-Ansible/blob/main/Docker-Module.md)

- Go to LAB to learn:  
  - [LAB: Important (Mostly Possible Used) Modules Sample Tasks](https://github.com/omerbsezer/Fast-Ansible/blob/main/Important-Modules-Sample-Tasks.md)
  
- All Modules in Ansible: 
  - [Cloud Modules (AWS, Azure, Digital Ocean, Docker, Google Cloud, OpenStack, Vmware)](https://docs.ansible.com/ansible/2.9/modules/list_of_cloud_modules.html)
  - [Clustering Modules (Kubernetes, ETCD)](https://docs.ansible.com/ansible/2.9/modules/list_of_clustering_modules.html)
  - [Command Modules (Command, Expect, Shell, Script)](https://docs.ansible.com/ansible/2.9/modules/list_of_commands_modules.html)
  - [Crypto Modules (OpenSSL, ACME)](https://docs.ansible.com/ansible/2.9/modules/list_of_crypto_modules.html)
  - [Database Modules (MySql, PostgreSql, MongoDB, ElasticSearch, Redis, Kibana, InfluxDB)](https://docs.ansible.com/ansible/2.9/modules/list_of_database_modules.html)
  - [Files Modules (Copy, LineIn, Find, File, Unarchive, Read_CSV)](https://docs.ansible.com/ansible/2.9/modules/list_of_files_modules.html)
  - [Messaging Modules (RabbitMQ)](https://docs.ansible.com/ansible/2.9/modules/list_of_messaging_modules.html)
  - [Monitoring Modules (Datadog, Grafana, Nagios, Zabbix)](https://docs.ansible.com/ansible/2.9/modules/list_of_monitoring_modules.html)
  - [Network Modules](https://docs.ansible.com/ansible/2.9/modules/list_of_network_modules.html)
  - [Notification Modules (MQTT, RabbitMq, Mattermost, Slack, Telegram)](https://docs.ansible.com/ansible/2.9/modules/list_of_notification_modules.html)
  - [Packaging Modules (Apt, Dnf, Homebrew, Snap, Package)](https://docs.ansible.com/ansible/2.9/modules/list_of_packaging_modules.html)
  - [Source Code Modules (Github, Gitlab, Bitbucket)](https://docs.ansible.com/ansible/2.9/modules/list_of_source_control_modules.html)
  - [System Modules (Authorized Keys, Cron, IpTables, Make, Sysctl,)](https://docs.ansible.com/ansible/2.9/modules/list_of_system_modules.html)
  - [Web Infrastructure Modules (Apache, Django, Jenkins, Jira, Ansible Tower)](https://docs.ansible.com/ansible/2.9/modules/list_of_web_infrastructure_modules.html)
  - [Windows Modules (Command, Chocotaley, Environment, File, Find, Firewall, Ping, Powershell, Regedit, Service, Shell, User, SNMP)](https://docs.ansible.com/ansible/2.9/modules/list_of_windows_modules.html)
  - [All Modules (Alphabet Order)](https://docs.ansible.com/ansible/2.9/modules/list_of_all_modules.html)

## Ansible Playbooks <a name="playbooks"></a>

- Instead of using Adhoc Commands, playbooks are used to store, manage easily (declerative way)
  
  ![image](https://user-images.githubusercontent.com/10358317/203531052-f9fc2527-06cc-4503-b042-bca5997b5bd0.png) 

- Playbooks are YAML files that include name, hosts (group name that is defined in inventoryfile), vars (variables that are used in playbooks) and tasks:

```
--- 
   name: install and configure DB
   hosts: testServer
   become: yes

   vars: 
     oracle_db_port_value : 1521
   
   tasks:
   -name: Install the Oracle DB
      yum: <code to install the DB>
    
   -name: Ensure the installed service is enabled and running
    service:
      name: <your service name>
```  

- Playbooks include hostname, user information, and tasks (with modules):

  ![image](https://user-images.githubusercontent.com/10358317/203531873-cf746f02-67cd-4d2d-98f8-fd7b4900b614.png) 
    
- Go to LAB to learn how playbook is created:
  - [LAB: Implement First Playbook](https://github.com/omerbsezer/Fast-Ansible/blob/main/Implement-First-Playbook.md)

- [LAB: Refactoring / Improving Playbook](https://github.com/omerbsezer/Fast-Ansible/blob/main/Refactoring-Playbook.md)

## Inventory File - Targeting Specific Nodes <a name="inventory"></a>

- For grouping the nodes (defining with names), we are using inventory file (nano inventory): 
``` 
[web_servers]
172.21.67.249

[database_servers]
172.21.75.98
``` 
- Go to LAB to create and use inventory file with playbook:
  - [LAB: Targeting Specific Nodes (Grouping)](https://github.com/omerbsezer/Fast-Ansible/blob/main/Targeting-Specific-Node.md)

## Tags <a name="tags"></a>
- With tags, some specific part of the code (playbook's play) could be run.

```
ansible-playbook --tags ubuntu --ask-become-pass site.yml
```

![image](https://user-images.githubusercontent.com/10358317/203546898-9dc1b0b5-929a-42ba-9cc4-7d919377aaba.png)

- Go to LAB to learn how to use tags:
  - [LAB: Adding Tags](https://github.com/omerbsezer/Fast-Ansible/blob/main/Tags.md)

## Managing Files <a name="files"></a>
- It is possible to transfer file from control node to all workers nodes, to download zip file from internet and to unzip files with playbooks.
  
  ![image](https://user-images.githubusercontent.com/10358317/203547001-fb106bbe-1171-4ca0-9d8d-5343907c6cf9.png)
  
- Go to LAB to learn how:
  - [LAB: Managing Files](https://github.com/omerbsezer/Fast-Ansible/blob/main/Managing-Files.md)

## Managing Services <a name="services"></a>
- It is possible to manage services (create, start, stop, restart, configure service file)

```
- name: start apache (Ubuntu)
  tags: ubuntu,apache,apache2
  service:
    name: apache2
    state: started
    enabled: yes
  when: ansible_distribution == "Ubuntu"
```

- Go to LAB to learn how:
  - [LAB: Managing Services](https://github.com/omerbsezer/Fast-Ansible/blob/main/Managing-Services.md)

## Adding Users <a name="users"></a>
- It is possible to manage users (add users, create SSH keys for users, add user as sudoers)

```
- name: create new user
  user:
    name: newuser111
    groups: root
```

- Go to LAB to learn how:
  - [LAB: Adding Users](https://github.com/omerbsezer/Fast-Ansible/blob/main/Adding-User.md)

## Roles <a name="roles"></a>
- Roles are defined to simplify, control your Ansible code like sofware code. 
- Roles are assigned to the group of nodes and roles help to define the task of these nodes. 

  ![image](https://user-images.githubusercontent.com/10358317/203546694-cb961a0b-e1e3-4e06-9f2d-0e70f1ef6cc8.png)

- Go to LAB to learn how:
  - [LAB: Roles](https://github.com/omerbsezer/Fast-Ansible/blob/main/Roles.md)

## Host Variables <a name="hostvariables"></a>
- It helps to define variables which are dependent to the hosts. 

  ![image](https://user-images.githubusercontent.com/10358317/203547741-9a52592b-7385-4e77-89ad-d8128ecf16b7.png)
  
  ![image](https://user-images.githubusercontent.com/10358317/203547770-7eef113c-c9ce-4043-9b02-588ba86fd747.png)

- Go to LAB to learn how:
  - [LAB: Host Variables](https://github.com/omerbsezer/Fast-Ansible/blob/main/Host-Variables.md)

## Handlers <a name="handlers"></a>
- To trigger/notify other Ansible code, handlers are used.

  ![image](https://user-images.githubusercontent.com/10358317/203547977-c5ced352-d323-4be6-bcd0-31a6d3df2735.png)

- Go to LAB to learn how:
  - [LAB: Handlers](https://github.com/omerbsezer/Fast-Ansible/blob/main/Handlers.md)

## Templates <a name="templates"></a>
- Ansible template module does two things: 
  - Replace the Jinja2 interpolation syntax variables present ({{ }}) in the template file with actual values.
  - Copy (scp) the file to the remote server.

- Jinja2 interpolation syntax variables in the playbook (ref: middlewareinventory):
  
  ![image](https://user-images.githubusercontent.com/10358317/203617129-308c53fb-7720-4d12-a55e-b4aa71dc9957.png)
  
- In Ansible {{ }} is the interpolation syntax whereas in shell script it is ${ }

- You can start your playbook like this with the variables at runtime.

```
ansible-playbook findtest.yaml -e "DIR=/apps/Tomcat FILEEXT=*.log DAYSOLD=30"
```

- Go to LAB to learn how:
  - [LAB: Templates](https://github.com/omerbsezer/Fast-Ansible/blob/main/Templates.md)

## Debugging <a name="debugging"></a>

- For verbosity, use -v, -vv (increase level), -vvv.

```
ansible all -m shell -a uptime -v
ansible all -m shell -a uptime -vv
ansible all -m shell -a uptime -vvv
```

## Details <a name="details"></a>

```
ansible-playbook <YAML> -f 10                       # Run 10 hosts parallel, default f=5 hosts parallel
ansible-playbook <YAML> -C                          # Test run
ansible-playbook <YAML> -C -D                       # Dry run
ansible-playbook <YAML> --user <username>           # Log in as username (or -u <username>)
ansible-playbook <YAML> --private-key <key>         # Log in using SSH key (usually in ~/.ssh) (or --key-file <key>)
ansible-playbook <YAML> --ssh-extra-args            # Pass extra command options to SSH
ansible-playbook <YAML> --vault-id <id>             # Use vault identity ID
ansible-playbook <YAML> --vault-password-file <key> # Use vault password file key
ansible-playbook <YAML> --ask-vault-pass            # Prompt for a vault password
ansible-playbook <YAML> --become                    # Escalate privileges
ansible-playbook <YAML> --ask-become-pass           # Prompt for a password for become
ansible-playbook <YAML> -l <host>                   # Run on single host
ansible-playbook <YAML> --list-hosts                # Run Infos
ansible-playbook <YAML> --list-tasks                # Run Infos
ansible-playbook <YAML> --syntax-check              # Syntax Check
ansible-playbook <YAML> --check                     # Run the playbook but don’t make changes
ansible-playbook <YAML> --diff                      # Show diffs for what changes are made

```

### Capture Shell Output
```
  tasks:
  - name: some shell
    register: sh_out
    ignore_errors: yes
    become_user: root
    shell: |
      find /

  - name: "Print stdout"
    debug:
      msg: "{{ sh_out.stdout.split('\n') }}"
  - name: "Print stderr"
    debug:
      msg: "{{ sh_out.stderr.split('\n') }}"
```

### Deleting files & directories
```
tasks:
- name: rm
  file:
    path: <some path>
    state: absent
    recurse: yes        # optional
```
## Other Useful Resources Related Ansible <a name="resource"></a>
- https://docs.ansible.com/ansible/2.9/
- Video Tutorial: https://www.youtube.com/watch?v=3RiVKs8GHYQ&list=PLT98CRl2KxKEUHie1m24-wkyHpEsa4Y70
- https://www.tutorialspoint.com/ansible/
- https://www.digitalocean.com/community/cheatsheets/how-to-use-ansible-cheat-sheet-guide
- https://www.middlewareinventory.com/blog/category/ansible/

## References <a name="references"></a>
- OpenSource: https://opensource.com/resources/what-ansible
- Video Tutorial: https://www.youtube.com/watch?v=3RiVKs8GHYQ&list=PLT98CRl2KxKEUHie1m24-wkyHpEsa4Y70
- https://www.kreyman.de/index.php/others/linux-kubernetes/213-ansible-verwendungsszenarien
- https://ibm.github.io/cloud-enterprise-examples/iac-conf-mgmt/ansible/
- Medium: https://medium.com/codex/automation-with-ansible-b39706ff777
- RedHat Presentation: https://speakerdeck.com/chrisshort/using-ansible-for-devops?slide=27
- https://www.middlewareinventory.com/blog/ansible-playbook-example/
- https://www.digitalocean.com/community/cheatsheets/how-to-use-ansible-cheat-sheet-guide
- https://www.middlewareinventory.com/blog/ansible-template-module-example/
