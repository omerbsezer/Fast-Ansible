# Fast-Ansible
This repo covers Ansible with LABs: Multipass, Ssh, Commands, Playbooks, Modules, etc.

**Keywords:** Ansible, Multi-PC Configuration.

# Hands-on LABs
- [LAB: Multipass - SSH Configuration]()
- [Ansible Commands Cheatsheet]()

# Table of Contents
- [Motivation](#motivation)
- [What is Ansible?](#whatIsAnsible)
    - [Kubernetes Architecture](#architecture)
- [Ansible Basic Commands](#modules)    
- [Ansible Modules](#modules)
- [Ansible Playbooks](#playbooks)
- [Targeting Specific Nodes](#)
- [Tags](#)
- [Managing Files](#)
- [Managing Services](#)
- [Adding Users](#)
- [Roles](#)
- [Host Variables & Handlers](#)
- [Templates](#)
- [Other Useful Resources Related Ansible](#resource)
- [References](#references)

## Motivation <a name="motivation"></a>
Why should we use Ansible? 

## What is Ansible? <a name="whatIsAnsible"></a>
- "Ansible is a software tool that provides simple but powerful automation for cross-platform computer support." (ref: Opensource.com)
- "In Ansible, there are two categories of computers: the control node and managed nodes. The control node is a computer that runs Ansible. There must be at least one control node, although a backup control node may also exist. A managed node is any device being managed by the control node." (ref: Opensource.com)
- "Ansible works by connecting to nodes (clients, servers, or whatever you're configuring) on a network, and then sending a small program called an Ansible module to that node. Ansible executes these modules over SSH and removes them when finished. The only requirement for this interaction is that your Ansible control node has login access to the managed nodes. SSH keys are the most common way to provide access, but other forms of authentication are also supported." (ref: Opensource.com)




## Other Useful Resources Related Ansible <a name="resource"></a>

## Reference <a name="references"></a>
- OpenSource: https://opensource.com/resources/what-ansible
- Video Tutorial: https://www.youtube.com/watch?v=3RiVKs8GHYQ&list=PLT98CRl2KxKEUHie1m24-wkyHpEsa4Y70
