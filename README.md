# Fast-Ansible
This repo covers Ansible with LABs: Multipass, Commands, Modules, Playbook, and details.

**Keywords:** Ansible, Multi-PC Configuration.

# Hands-on LABs
- [LAB: Multipass-SSH Configuration (Create Ansible Test Environment)](https://github.com/omerbsezer/Fast-Ansible/blob/main/Multipass-SSH-Configuration.md)
- [LAB: Install Ansible and Test Basic Ansible Commands](https://github.com/omerbsezer/Fast-Ansible/blob/main/Install-Ansible-Basic-Commands.md)
- [LAB: Implement First Playbook](https://github.com/omerbsezer/Fast-Ansible/blob/main/Implement-First-Playbook.md)
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
- [Ansible Commands Cheatsheet]()

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
- [Other Useful Resources Related Ansible](#resource)
- [References](#references)

## Motivation <a name="motivation"></a>

Why should we use / learn Ansible? 

- It automates tasks and commands to manage multiple nodes (servers, PCs).
- Ansible is a state-of-the-art automation tool. Many companies use it.
- Ansible can be used both on-premises and cloud environment.
- Commands, tasks, codes turn into the Infrastructure As Code (IaC).
  - With IaC, tasks is savable, versionable, repetable and testable.
  - With IaC, desired configuration is defined as 'Declerative Way'.
- **Agentless:** On the worker node, any agent app is not required to run.
- It runs tasks both on Linux and Windows PCs.
- It has well-designed documentation (https://docs.ansible.com)
- Ansible uses SSH to communicate with other nodes.
- Ansible handles many tasks using its modules and easy to integrate for other tools/apps (e.g. [Terraform](https://www.terraform.io/)).

![image](https://user-images.githubusercontent.com/10358317/202701707-b160e35c-7a05-43e8-93c7-b626c8054aa9.png) (ref: medium)

- Ansible also is used by other important applications (e.g. Open Source Gating CI Tools: [Zuul-CI](https://zuul-ci.org/))


![image](https://user-images.githubusercontent.com/10358317/202699093-62fcc145-c023-43ed-af51-0866393f0701.png) (ref: kreyman.de)

- Ansible is used for configuration management on the nodes, Terraform is provisioning tool that uses to create/provision Cloud Infrastructure objects/items (e.g. Virtual Private Cloud, Virtual Machines, Subnets, etc.)


![image](https://user-images.githubusercontent.com/10358317/202700302-d651cf08-dd55-44ea-a88c-8ee4186d9438.png) (ref: ibm.github.io)


## What is Ansible? <a name="whatIsAnsible"></a>
- "Ansible is a software tool that provides simple but powerful automation for cross-platform computer support." (ref: Opensource.com)
- "In Ansible, there are two categories of computers: the control node and managed nodes. The control node is a computer that runs Ansible. There must be at least one control node, although a backup control node may also exist. A managed node is any device being managed by the control node." (ref: Opensource.com)
- "Ansible works by connecting to nodes (clients, servers, or whatever you're configuring) on a network, and then sending a small program called an Ansible module to that node. Ansible executes these modules over SSH and removes them when finished. The only requirement for this interaction is that your Ansible control node has login access to the managed nodes. SSH keys are the most common way to provide access, but other forms of authentication are also supported." (ref: Opensource.com)

## How Ansible Works? <a name="howAnsibleWorks"></a>

![image](https://user-images.githubusercontent.com/10358317/202703050-da88ea06-3416-4559-82f3-7ae8c709ce72.png) (ref: medium)

## Creating LAB Environment <a name="labEnvironment"></a>

- [LAB: Multipass-SSH Configuration (Create Ansible Test Environment)](https://github.com/omerbsezer/Fast-Ansible/blob/main/Multipass-SSH-Configuration.md)

## Ansible Basic (Ad-Hoc) Commands <a name="commands"></a>

- [LAB: Install Ansible and Test Basic Ansible Commands](https://github.com/omerbsezer/Fast-Ansible/blob/main/Install-Ansible-Basic-Commands.md)

## Ansible Modules <a name="modules"></a>

https://docs.ansible.com/ansible/2.9/modules/list_of_all_modules.html

## Ansible Playbooks <a name="playbooks"></a>

- [LAB: Implement First Playbook](https://github.com/omerbsezer/Fast-Ansible/blob/main/Implement-First-Playbook.md)
- [LAB: Refactoring / Improving Playbook](https://github.com/omerbsezer/Fast-Ansible/blob/main/Refactoring-Playbook.md)

## Inventory File - Targeting Specific Nodes <a name="inventory"></a>

- [LAB: Targeting Specific Nodes (Grouping)](https://github.com/omerbsezer/Fast-Ansible/blob/main/Targeting-Specific-Node.md)


## Tags <a name="tags"></a>

- [LAB: Adding Tags](https://github.com/omerbsezer/Fast-Ansible/blob/main/Tags.md)

## Managing Files <a name="files"></a>

- [LAB: Managing Files](https://github.com/omerbsezer/Fast-Ansible/blob/main/Managing-Files.md)

## Managing Services <a name="services"></a>

- [LAB: Managing Services](https://github.com/omerbsezer/Fast-Ansible/blob/main/Managing-Services.md)

## Adding Users <a name="users"></a>

- [LAB: Adding Users](https://github.com/omerbsezer/Fast-Ansible/blob/main/Adding-User.md)

## Roles <a name="roles"></a>

- [LAB: Roles](https://github.com/omerbsezer/Fast-Ansible/blob/main/Roles.md)

## Host Variables <a name="hostvariables"></a>

- [LAB: Host Variables](https://github.com/omerbsezer/Fast-Ansible/blob/main/Host-Variables.md)

## Handlers <a name="handlers"></a>

- [LAB: Handlers](https://github.com/omerbsezer/Fast-Ansible/blob/main/Handlers.md)

## Templates <a name="templates"></a>

- [LAB: Templates](https://github.com/omerbsezer/Fast-Ansible/blob/main/Templates.md)

## Other Useful Resources Related Ansible <a name="resource"></a>

## Reference <a name="references"></a>
- OpenSource: https://opensource.com/resources/what-ansible
- Video Tutorial: https://www.youtube.com/watch?v=3RiVKs8GHYQ&list=PLT98CRl2KxKEUHie1m24-wkyHpEsa4Y70
- https://www.kreyman.de/index.php/others/linux-kubernetes/213-ansible-verwendungsszenarien
- https://ibm.github.io/cloud-enterprise-examples/iac-conf-mgmt/ansible/
- Medium: https://medium.com/codex/automation-with-ansible-b39706ff777
