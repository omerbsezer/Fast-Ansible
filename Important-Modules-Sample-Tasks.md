## LAB: Important (Mostly Possible Used) Modules Sample Tasks

This scenario shows the important (Mostly Possible Used) modules sample tasks:
- [File Module](#file)
- [Lineinfile Module](#lineinfile)
- [Unarchive Module](#unarchive)
- [Command Module](#command)
- [Shell Module](#shell)
- [Windows PsExec Module](#psexec)

### Prerequisite

- You should have a look following lab, nodes that are created in that LAB, are using in ansible commands
  - [LAB: Multipass-SSH Configuration (Create Ansible Test Environment)](https://github.com/omerbsezer/Fast-Ansible/blob/main/Multipass-SSH-Configuration.md)

### File Module <a name="file"></a>

Details: https://docs.ansible.com/ansible/2.9/modules/list_of_files_modules.html

```
- name: Change file ownership, group and permissions
  file:
    path: /etc/foo.conf
    owner: foo
    group: foo
    mode: '0644'

- name: Give insecure permissions to an existing file
  file:
    path: /work
    owner: root
    group: root
    mode: '1777'

- name: Create a symbolic link
  file:
    src: /file/to/link/to
    dest: /path/to/symlink
    owner: foo
    group: foo
    state: link

- name: Create two hard links
  file:
    src: '/tmp/{{ item.src }}'
    dest: '{{ item.dest }}'
    state: hard
  loop:
    - { src: x, dest: y }
    - { src: z, dest: k }

- name: Touch a file, using symbolic modes to set the permissions (equivalent to 0644)
  file:
    path: /etc/foo.conf
    state: touch
    mode: u=rw,g=r,o=r

- name: Touch the same file, but add/remove some permissions
  file:
    path: /etc/foo.conf
    state: touch
    mode: u+rw,g-wx,o-rwx

- name: Touch again the same file, but dont change times this makes the task idempotent
  file:
    path: /etc/foo.conf
    state: touch
    mode: u+rw,g-wx,o-rwx
    modification_time: preserve
    access_time: preserve

- name: Create a directory if it does not exist
  file:
    path: /etc/some_directory
    state: directory
    mode: '0755'

- name: Update modification and access time of given file
  file:
    path: /etc/some_file
    state: file
    modification_time: now
    access_time: now

- name: Set access time based on seconds from epoch value
  file:
    path: /etc/another_file
    state: file
    access_time: '{{ "%Y%m%d%H%M.%S" | strftime(stat_var.stat.atime) }}'

- name: Recursively change ownership of a directory
  file:
    path: /etc/foo
    state: directory
    recurse: yes
    owner: foo
    group: foo

- name: Remove file (delete file)
  file:
    path: /etc/foo.txt
    state: absent

- name: Recursively remove directory
  file:
    path: /etc/foo
    state: absent
``` 

### LineInFile Module <a name="lineinfile"></a>

Details: https://docs.ansible.com/ansible/2.9/modules/lineinfile_module.html#lineinfile-module

``` 
# NOTE: Before 2.3, option 'dest', 'destfile' or 'name' was used instead of 'path'
- name: Ensure SELinux is set to enforcing mode
  lineinfile:
    path: /etc/selinux/config
    regexp: '^SELINUX='
    line: SELINUX=enforcing

- name: Make sure group wheel is not in the sudoers configuration
  lineinfile:
    path: /etc/sudoers
    state: absent
    regexp: '^%wheel'

- name: Replace a localhost entry with our own
  lineinfile:
    path: /etc/hosts
    regexp: '^127\.0\.0\.1'
    line: 127.0.0.1 localhost
    owner: root
    group: root
    mode: '0644'

- name: Ensure the default Apache port is 8080
  lineinfile:
    path: /etc/httpd/conf/httpd.conf
    regexp: '^Listen '
    insertafter: '^#Listen '
    line: Listen 8080

- name: Ensure we have our own comment added to /etc/services
  lineinfile:
    path: /etc/services
    regexp: '^# port for http'
    insertbefore: '^www.*80/tcp'
    line: '# port for http by default'

- name: Add a line to a file if the file does not exist, without passing regexp
  lineinfile:
    path: /tmp/testfile
    line: 192.168.1.99 foo.lab.net foo
    create: yes

# NOTE: Yaml requires escaping backslashes in double quotes but not in single quotes
- name: Ensure the JBoss memory settings are exactly as needed
  lineinfile:
    path: /opt/jboss-as/bin/standalone.conf
    regexp: '^(.*)Xms(\\d+)m(.*)$'
    line: '\1Xms${xms}m\3'
    backrefs: yes

# NOTE: Fully quoted because of the ': ' on the line. See the Gotchas in the YAML docs.
- name: Validate the sudoers file before saving
  lineinfile:
    path: /etc/sudoers
    state: present
    regexp: '^%ADMIN ALL='
    line: '%ADMIN ALL=(ALL) NOPASSWD: ALL'
    validate: /usr/sbin/visudo -cf %s
``` 

### Unarchive Module <a name="unarchive"></a>

Details: https://docs.ansible.com/ansible/2.9/modules/unarchive_module.html#unarchive-module

```
- name: Extract foo.tgz into /var/lib/foo
  unarchive:
    src: foo.tgz
    dest: /var/lib/foo

- name: Unarchive a file that is already on the remote machine
  unarchive:
    src: /tmp/foo.zip
    dest: /usr/local/bin
    remote_src: yes

- name: Unarchive a file that needs to be downloaded (added in 2.0)
  unarchive:
    src: https://example.com/example.zip
    dest: /usr/local/bin
    remote_src: yes

- name: Unarchive a file with extra options
  unarchive:
    src: /tmp/foo.zip
    dest: /usr/local/bin
    extra_opts:
    - --transform
    - s/^xxx/yyy/
```    
    
### Command Module <a name="command"></a>

Details: https://docs.ansible.com/ansible/2.9/modules/command_module.html#command-module

```    
- name: return motd to registered var
  command: cat /etc/motd
  register: mymotd

- name: Run command if /path/to/database does not exist (without 'args' keyword).
  command: /usr/bin/make_database.sh db_user db_name creates=/path/to/database

# 'args' is a task keyword, passed at the same level as the module
- name: Run command if /path/to/database does not exist (with 'args' keyword).
  command: /usr/bin/make_database.sh db_user db_name
  args:
    creates: /path/to/database

# 'cmd' is module parameter
- name: Run command if /path/to/database does not exist (with 'cmd' parameter).
  command:
    cmd: /usr/bin/make_database.sh db_user db_name
    creates: /path/to/database

- name: Change the working directory to somedir/ and run the command as db_owner if /path/to/database does not exist.
  command: /usr/bin/make_database.sh db_user db_name
  become: yes
  become_user: db_owner
  args:
    chdir: somedir/
    creates: /path/to/database

# 'argv' is a parameter, indented one level from the module
- name: Use 'argv' to send a command as a list - leave 'command' empty
  command:
    argv:
      - /usr/bin/make_database.sh
      - Username with whitespace
      - dbname with whitespace

- name: safely use templated variable to run command. Always use the quote filter to avoid injection issues.
  command: cat {{ myfile|quote }}
  register: myoutput    
``` 

### Shell Module <a name="shell"></a>

Details: https://docs.ansible.com/ansible/2.9/modules/shell_module.html#shell-module

```
- name: Execute the command in remote shell; stdout goes to the specified file on the remote.
  shell: somescript.sh >> somelog.txt

- name: Change the working directory to somedir/ before executing the command.
  shell: somescript.sh >> somelog.txt
  args:
    chdir: somedir/

# You can also use the 'args' form to provide the options.
- name: This command will change the working directory to somedir/ and will only run when somedir/somelog.txt doesn't exist.
  shell: somescript.sh >> somelog.txt
  args:
    chdir: somedir/
    creates: somelog.txt

# You can also use the 'cmd' parameter instead of free form format.
- name: This command will change the working directory to somedir/.
  shell:
    cmd: ls -l | grep log
    chdir: somedir/

- name: Run a command that uses non-posix shell-isms (in this example /bin/sh doesn't handle redirection and wildcards together but bash does)
  shell: cat < /tmp/*txt
  args:
    executable: /bin/bash

- name: Run a command using a templated variable (always use quote filter to avoid injection)
  shell: cat {{ myfile|quote }}

# You can use shell to run other executables to perform actions inline
- name: Run expect to wait for a successful PXE boot via out-of-band CIMC
  shell: |
    set timeout 300
    spawn ssh admin@{{ cimc_host }}

    expect "password:"
    send "{{ cimc_password }}\n"

    expect "\n{{ cimc_name }}"
    send "connect host\n"

    expect "pxeboot.n12"
    send "\n"

    exit 0
  args:
    executable: /usr/bin/expect
  delegate_to: localhost

# Disabling warnings
- name: Using curl to connect to a host via SOCKS proxy (unsupported in uri). Ordinarily this would throw a warning.
  shell: curl --socks5 localhost:9000 http://www.ansible.com
  args:
    warn: no
```

### Windows PsExec Module <a name="psexec"></a>

Details: https://docs.ansible.com/ansible/2.9/modules/psexec_module.html#psexec-module

```
- name: Run a cmd.exe command
  psexec:
    hostname: server
    connection_username: username
    connection_password: password
    executable: cmd.exe
    arguments: /c echo Hello World

- name: Run a PowerShell command
  psexec:
    hostname: server.domain.local
    connection_username: username@DOMAIN.LOCAL
    connection_password: password
    executable: powershell.exe
    arguments: Write-Host Hello World

- name: Send data through stdin
  psexec:
    hostname: 192.168.1.2
    connection_username: username
    connection_password: password
    executable: powershell.exe
    arguments: '-'
    stdin: |
      Write-Host Hello World
      Write-Error Error Message
      exit 0

- name: Run the process as a different user
  psexec:
    hostname: server
    connection_user: username
    connection_password: password
    executable: whoami.exe
    arguments: /all
    process_username: anotheruser
    process_password: anotherpassword

- name: Run the process asynchronously
  psexec:
    hostname: server
    connection_username: username
    connection_password: password
    executable: cmd.exe
    arguments: /c rmdir C:\temp
    asynchronous: yes

- name: Use Kerberos authentication for the connection (requires smbprotocol[kerberos])
  psexec:
    hostname: host.domain.local
    connection_username: user@DOMAIN.LOCAL
    executable: C:\some\path\to\executable.exe
    arguments: /s

- name: Disable encryption to work with WIndows 7/Server 2008 (R2)
  psexec:
    hostanme: windows-pc
    connection_username: Administrator
    connection_password: Password01
    encrypt: no
    integrity_level: elevated
    process_username: Administrator
    process_password: Password01
    executable: powershell.exe
    arguments: (New-Object -ComObject Microsoft.Update.Session).CreateUpdateInstaller().IsBusy

- name: Download and run ConfigureRemotingForAnsible.ps1 to setup WinRM
  psexec:
    hostname: '{{ hostvars[inventory_hostname]["ansible_host"] | default(inventory_hostname) }}'
    connection_username: '{{ ansible_user }}'
    connection_password: '{{ ansible_password }}'
    encrypt: yes
    executable: powershell.exe
    arguments: '-'
    stdin: |
      $ErrorActionPreference = "Stop"
      $sec_protocols = [Net.ServicePointManager]::SecurityProtocol -bor [Net.SecurityProtocolType]::SystemDefault
      $sec_protocols = $sec_protocols -bor [Net.SecurityProtocolType]::Tls12
      [Net.ServicePointManager]::SecurityProtocol = $sec_protocols
      $url = "https://github.com/ansible/ansible/raw/devel/examples/scripts/ConfigureRemotingForAnsible.ps1"
      Invoke-Expression ((New-Object Net.WebClient).DownloadString($url))
      exit
  delegate_to: localhost
```  
