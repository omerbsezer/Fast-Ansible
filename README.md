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
Why should we use Ansible? 

## What is Ansible? <a name="whatIsAnsible"></a>
- "Ansible is a software tool that provides simple but powerful automation for cross-platform computer support." (ref: Opensource.com)
- "In Ansible, there are two categories of computers: the control node and managed nodes. The control node is a computer that runs Ansible. There must be at least one control node, although a backup control node may also exist. A managed node is any device being managed by the control node." (ref: Opensource.com)
- "Ansible works by connecting to nodes (clients, servers, or whatever you're configuring) on a network, and then sending a small program called an Ansible module to that node. Ansible executes these modules over SSH and removes them when finished. The only requirement for this interaction is that your Ansible control node has login access to the managed nodes. SSH keys are the most common way to provide access, but other forms of authentication are also supported." (ref: Opensource.com)

## Creating LAB Environment <a name="labEnvironment"></a>

- [LAB: Multipass-SSH Configuration (Create Ansible Test Environment)](https://github.com/omerbsezer/Fast-Ansible/blob/main/Multipass-SSH-Configuration.md)

## Ansible Basic (Ad-Hoc) Commands <a name="commands"></a>

- [LAB: Install Ansible and Test Basic Ansible Commands](https://github.com/omerbsezer/Fast-Ansible/blob/main/Install-Ansible-Basic-Commands.md)

## Ansible Modules <a name="modules"></a>


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
