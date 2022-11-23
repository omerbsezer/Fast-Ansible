## LAB: Multipass-SSH-Configuration (Create Ansible Test Environment)

This scenario shows:
- how to install multipass
- how to create control node, managed nodes
- how to configure ssh between control node and managed nodes



### Steps

- "Multipass is a mini-cloud on your workstation using native hypervisors of all the supported plaforms (Windows, macOS and Linux)"
- Fast to install and to use.
- **Link:** https://multipass.run/
- Install on Linux, Windows and MacOs: https://multipass.run/install
- After installing, we can create VMs on our local machine.

``` 
# creating controlnode, managed nodes (node1, node2, etc.)
# -c => cpu, -m => memory, -d => disk space
multipass launch --name controlnode -c 2 -m 2G -d 10G   
multipass launch --name node1 -c 2 -m 2G -d 10G
multipass launch --name node2 -c 2 -m 2G -d 10G
multipass list
``` 

![image](https://user-images.githubusercontent.com/10358317/201082045-b90b645b-c6b5-4d8f-9bd6-4bb09c0f0ea7.png)

- Connect VMs by opening shells

``` 
# get shell on controlnode
multipass shell controlnode
# get shell on node1, on different terminal
multipass shell node1
# get shell on node2, on different terminal
multipass shell node2
``` 

![image](https://user-images.githubusercontent.com/10358317/201082458-97b41058-5389-4301-b1e2-06a1b9d3a4ba.png)

``` 
sudo apt update
sudo apt install net-tools
# to see IPs
ifconfig
``` 

- Create ssh public key on control plane
- Copy the public key from control plane

``` 
> on controlnode
ssh-keygen (no password, enter 3 times)
cat ~/.ssh/id_rsa.pub (copy the value)
``` 

![image](https://user-images.githubusercontent.com/10358317/201083201-8e0a9bfb-8001-429e-881f-d38a7c970015.png)

- Paste copied public key (control plane) into the authorized_keys in each managed nodes.

``` 
cd .ssh (on each workers)
nano authorized_keys 
> Paste keys from controlnode (hence managed nodes know the controlnode IP and public key, controlnode can connect it)
``` 

![image](https://user-images.githubusercontent.com/10358317/201083610-d4141690-d5d7-4f9c-90ba-dbfb6743b2d1.png)

- List all VMs to get IPs

```
multipass list
```

![image](https://user-images.githubusercontent.com/10358317/201084356-c34f3629-7e86-4e15-9cad-3361b5a49f34.png)

- SSH from controlplane to node1

```
ssh <IP>
or 
ssh <username>@<IP>
```

![image](https://user-images.githubusercontent.com/10358317/201084577-1028dc59-be04-4cb4-982b-f3dca1ea6251.png)

### References
- https://techsparx.com/linux/multipass/enable-ssh.html
