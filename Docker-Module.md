## LAB: Playing Docker Module

This scenario shows:
- how to install docker
- how to pull and list docker images
- how to run, stop, remove docker containers.

### Prerequisite

- You should have a look following lab, nodes that are created in that LAB, are using in ansible commands
  - [LAB: Multipass-SSH Configuration (Create Ansible Test Environment)](https://github.com/omerbsezer/Fast-Ansible/blob/main/Multipass-SSH-Configuration.md)

### Steps

- Create docker_play.yml (nano docker_play.yml)
- This file provides to install Docker, pull ubuntu image 

``` 
---
- hosts: all
  become: true
  vars:
    container_count: 4
    default_container_name: docker
    default_container_image: ubuntu
    default_container_command: sleep 1d

  tasks:
    - name: Install aptitude
      tags: install
      apt:
        name: aptitude
        state: latest
        update_cache: true

    - name: Install required system packages
      tags: install
      apt:
        pkg:
          - apt-transport-https
          - ca-certificates
          - curl
          - software-properties-common
          - python3-pip
          - virtualenv
          - python3-setuptools
        state: latest
        update_cache: true

    - name: Add Docker GPG apt Key
      tags: install
      apt_key:
        url: https://download.docker.com/linux/ubuntu/gpg
        state: present

    - name: Add Docker Repository
      tags: install
      apt_repository:
        repo: deb https://download.docker.com/linux/ubuntu focal stable
        state: present
        
    - name: Update apt and install docker-ce
      tags: install
      apt:
        name: docker-ce
        state: latest
        update_cache: true

    - name: Install Docker Module for Python
      tags: install
      pip:
        name: docker

    - name: Pull default Docker image
      tags: pull
      docker_image:
        name: "{{ default_container_image }}"
        source: pull

    - name: Run default containers
      tags: run
      docker_container:
        name: "{{ default_container_name }}{{ item }}"
        image: "{{ default_container_image }}"
        command: "{{ default_container_command }}"
        state: present
      with_sequence: count={{ container_count }}

    - name: docker container ls -a
      tags: container_ls
      become: True
      shell:
        "docker container ls -a"
      register: container_info

    - name: docker container ls -a result
      tags: container_ls
      debug:
        msg: "{{container_info.stdout_lines}}"

    - name: docker images
      tags: image_ls
      become: True
      shell:
        "docker images"
      register: image_info
      
    - name: docker images result
      tags: image_ls
      debug:
        msg: "{{image_info.stdout_lines}}"

    - name: Stop containers
      tags: stop
      docker_container:
        name: "{{ default_container_name }}{{ item }}"
        state: stopped
      with_sequence: count={{ container_count }}

    - name: Remove containers
      tags: remove
      docker_container:
        name: "{{ default_container_name }}{{ item }}"
        state: absent
      with_sequence: count={{ container_count }}  
``` 

- Run following command to run all, but we should use tags to run specific commands:

```
ansible-playbook docker_play.yml
``` 

![image](https://user-images.githubusercontent.com/10358317/203569878-7eec129a-82ce-47a4-90b4-2f11a08337f5.png)


```
ssh 172.21.67.249
sudo docker container ls -a
```

![image](https://user-images.githubusercontent.com/10358317/203570423-d9721ceb-6869-439c-842d-66112fc931ec.png)

- To install docker on nodes:
```
ansible-playbook docker_play.yml --tags install
```

- To run (sudo docker container ls -a):
```
ansible-playbook docker_play.yml --tags container_ls
```

![image](https://user-images.githubusercontent.com/10358317/203594440-46b99a95-f4b5-4f8b-9d10-93d5cd8eddd5.png)

- To run (sudo docker images):
```
ansible-playbook docker_play.yml --tags image_ls
```

![image](https://user-images.githubusercontent.com/10358317/203594583-4edac3c5-50b3-4ee3-8172-2e6413adf094.png)

- To stop docker containers (in this case, containers are not up, their status is 'created'):

```
ansible-playbook docker_play.yml --tags stop
```

- To remove docker containers: 

```
ansible-playbook docker_play.yml --tags remove
```

![image](https://user-images.githubusercontent.com/10358317/203595029-7e9c6d1a-2b5f-4881-9d9e-9c60b68cb0e0.png)


### Sample Docker Tasks:

- Sample Docker Tasks are taken from here: 
  - https://docs.ansible.com/ansible/2.9/modules/docker_container_module.html#docker-container-module

```
- name: Create a data container
  docker_container:
    name: mydata
    image: busybox
    volumes:
      - /data

- name: Re-create a redis container
  docker_container:
    name: myredis
    image: redis
    command: redis-server --appendonly yes
    state: present
    recreate: yes
    exposed_ports:
      - 6379
    volumes_from:
      - mydata

- name: Restart a container
  docker_container:
    name: myapplication
    image: someuser/appimage
    state: started
    restart: yes
    links:
     - "myredis:aliasedredis"
    devices:
     - "/dev/sda:/dev/xvda:rwm"
    ports:
     - "8080:9000"
     - "127.0.0.1:8081:9001/udp"
    env:
        SECRET_KEY: "ssssh"
        # Values which might be parsed as numbers, booleans or other types by the YAML parser need to be quoted
        BOOLEAN_KEY: "yes"
```

```
- name: Container present
  docker_container:
    name: mycontainer
    state: present
    image: ubuntu:14.04
    command: sleep infinity

- name: Stop a container
  docker_container:
    name: mycontainer
    state: stopped

- name: Start 4 load-balanced containers
  docker_container:
    name: "container{{ item }}"
    recreate: yes
    image: someuser/anotherappimage
    command: sleep 1d
  with_sequence: count=4

- name: remove container
  docker_container:
    name: ohno
    state: absent
```

```
- name: Syslogging output
  docker_container:
    name: myservice
    image: busybox
    log_driver: syslog
    log_options:
      syslog-address: tcp://my-syslog-server:514
      syslog-facility: daemon
      # NOTE: in Docker 1.13+ the "syslog-tag" option was renamed to "tag" for
      # older docker installs, use "syslog-tag" instead
      tag: myservice

- name: Create db container and connect to network
  docker_container:
    name: db_test
    image: "postgres:latest"
    networks:
      - name: "{{ docker_network_name }}"

- name: Start container, connect to network and link
  docker_container:
    name: sleeper
    image: ubuntu:14.04
    networks:
      - name: TestingNet
        ipv4_address: "172.1.1.100"
        aliases:
          - sleepyzz
        links:
          - db_test:db
      - name: TestingNet2
```

```
- name: Start a container with a command
  docker_container:
    name: sleepy
    image: ubuntu:14.04
    command: ["sleep", "infinity"]

- name: Add container to networks
  docker_container:
    name: sleepy
    networks:
      - name: TestingNet
        ipv4_address: 172.1.1.18
        links:
          - sleeper
      - name: TestingNet2
        ipv4_address: 172.1.10.20

- name: Update network with aliases
  docker_container:
    name: sleepy
    networks:
      - name: TestingNet
        aliases:
          - sleepyz
          - zzzz
```

```
- name: Remove container from one network
  docker_container:
    name: sleepy
    networks:
      - name: TestingNet2
    purge_networks: yes

- name: Remove container from all networks
  docker_container:
    name: sleepy
    purge_networks: yes

- name: Start a container and use an env file
  docker_container:
    name: agent
    image: jenkinsci/ssh-slave
    env_file: /var/tmp/jenkins/agent.env

- name: Create a container with limited capabilities
  docker_container:
    name: sleepy
    image: ubuntu:16.04
    command: sleep infinity
    capabilities:
      - sys_time
    cap_drop:
      - all
```

```
- name: Finer container restart/update control
  docker_container:
    name: test
    image: ubuntu:18.04
    env:
      arg1: "true"
      arg2: "whatever"
    volumes:
      - /tmp:/tmp
    comparisons:
      image: ignore   # don't restart containers with older versions of the image
      env: strict   # we want precisely this environment
      volumes: allow_more_present   # if there are more volumes, that's ok, as long as `/tmp:/tmp` is there

- name: Finer container restart/update control II
  docker_container:
    name: test
    image: ubuntu:18.04
    env:
      arg1: "true"
      arg2: "whatever"
    comparisons:
      '*': ignore  # by default, ignore *all* options (including image)
      env: strict   # except for environment variables; there, we want to be strict

- name: Start container with healthstatus
  docker_container:
    name: nginx-proxy
    image: nginx:1.13
    state: started
    healthcheck:
      # Check if nginx server is healthy by curl'ing the server.
      # If this fails or timeouts, the healthcheck fails.
      test: ["CMD", "curl", "--fail", "http://nginx.host.com"]
      interval: 1m30s
      timeout: 10s
      retries: 3
      start_period: 30s

- name: Remove healthcheck from container
  docker_container:
    name: nginx-proxy
    image: nginx:1.13
    state: started
    healthcheck:
      # The "NONE" check needs to be specified
      test: ["NONE"]
```

```
- name: start container with block device read limit
  docker_container:
    name: test
    image: ubuntu:18.04
    state: started
    device_read_bps:
      # Limit read rate for /dev/sda to 20 mebibytes per second
      - path: /dev/sda
        rate: 20M
    device_read_iops:
      # Limit read rate for /dev/sdb to 300 IO per second
      - path: /dev/sdb
        rate: 300
```

### All Docker Modules:

- https://docs.ansible.com/ansible/2.9/modules/list_of_cloud_modules.html#docker

### Reference

- https://www.digitalocean.com/community/tutorials/how-to-use-ansible-to-install-and-set-up-docker-on-ubuntu-20-04
- https://docs.ansible.com/ansible/2.9/modules/docker_container_module.html#docker-container-module
