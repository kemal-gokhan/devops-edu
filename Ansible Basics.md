# Ansible

<u>Ansible Control Manager - Ansible Targets</u>

playbook.yml

keep watch - list or dictionary on yaml file.

Ansible has many modules.

**Just linux now and work with SSH**

`useradd ansadmin`

`passwd ansadmin`

`visudo`

User Conf

Sudoers file will change with your user created with nopasswd parameter.
ALL=(ALL) NOPASSWD: ALL

`pip install ansible`

`touch hosts`

`ansible modules ad-hoc commands`

`ansible all -m user "name=john" -b` (add user all servers called john)

`ansible all -m setup (show setup)`

##### Gathered facts

Ansible facts are data gathered about target nodes (host nodes to be configured) and returned back to controller nodes. Once you create file ansible also collect the data related ansible pool in the remote system. So you can skip with gather_facts: no

The final situation is succeed or changed.

Inventory https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html 

Ansible works against multiple managed nodes or “hosts” in your infrastructure at the same time, using a list or group of lists known as inventory. Once your inventory is defined, you use [patterns](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#intro-patterns) to select the hosts or groups you want Ansible to run against.

 Multiple installation ->  name: ["apache2","wget","zip"]

```
shell commands
untar, chdir 
```

*tags for playing just case you mentioned.* `ignore_errors: yes or no`

### Location

`etc/ansible/hosts`

ansible -i inventorydirectory

### To view detail

ansible.cfg

you can group the instances on ansible     

ansible rhel -m ping 

create own ansible.cfg then you can manage inventory and host files.

Syntax

ansible is iterativ and ansible-playbook is decalarative method.

```
-
    name: 'Execute a command to display hosts file on localhost'
    hosts: localhost
    tasks:
        -
            name: 'Execute a command to display hosts file'
            command: 'cat /etc/hosts'

-
    name: 'Execute two commands on web_node1'
    hosts: boston_nodes
    tasks:
        -
            name: 'Execute a date command'
            command: date
        -
            name: 'Execute a command to display hosts file'
            command: 'cat /etc/hosts'
```

### Ansible Modules

- System
- Commands
- Files
- Database
- Cloud
- Windows
- More

```
-
    name: 'Execute a script on all web server nodes'
    hosts: web_nodes
    tasks:
        -
            name: 'Execute a script on all web server nodes'
            script: /tmp/install_script.sh
-
    name: 'Execute a script on all web server nodes'
    hosts: web_nodes
    tasks:
        -
            name: 'Execute a script'
            script: /tmp/install_script.sh
        -
            name: 'Start httpd service'
            service: 'name=httpd state=started'
```

```
-
    name: 'Execute a script on all web server nodes and start httpd service'
    hosts: web_nodes
    tasks:
        -
            name: 'Update entry into /etc/resolv.conf'
            lineinfile:
                path: /etc/resolv.conf
                line: 'nameserver 10.1.250.10'
        -
            name: 'Execute a script'
            script: /tmp/install_script.sh
        -
            name: 'Start httpd service'
            service:
                name: httpd
                state: present
```

"Ansible **Lineinfile **is **a module of Ansible that is used to modify the particular line in a file**. It is useful to add a new line, modify a line, replace a line, and remove an existing line in a file if it finds a specific text."

```
-
    name: 'Execute a script on all web server nodes and start httpd service'
    hosts: web_nodes
    tasks:
        -
            name: 'Update entry into /etc/resolv.conf'
            lineinfile:
                path: /etc/resolv.conf
                line: 'nameserver 10.1.250.10'
        -
            name: 'Create a new web user'
            user:
                name: web_user
                uid: 1040
                group: developers    

        -
            name: 'Execute a script'
            script: /tmp/install_script.sh
        -
            name: 'Start httpd service'
            service:
                name: httpd
                state: present
```

### Variables

```
ansible_connection=ssh
vars:
  dns_server: 10.1.250.1
```

```
  -
    name: 'Update nameserver entry into resolv.conf file on localhost'
    hosts: localhost
    tasks:
        -
            name: 'Update nameserver entry into resolv.conf file'
            lineinfile:
                path: /etc/resolv.conf
                line: 'nameserver {{ nameserver }}
```

### Vars Definition:

```
- hosts: webservers
  vars:
    http_port: 80
```

```
- hosts: all
 remote_user: root
 vars:
   favcolor: blue
 vars_files:
   - /vars/external_vars.yml
```

`ansible-playbook release.yml --extra-vars "version=1.23.45 other_variable=foo"`

### Conditional -when

when parameter is used for conditional statement.
or is also can be usable.

register: and when with -1 result!

```
-
    name: 'Add name server entry if not already entered'
    hosts: localhost
    tasks:
        -
            shell: 'cat /etc/resolv.conf'
            register: command_output
        -
            shell: 'echo "nameserver 10.0.250.10" >> /etc/resolv.conf'
            when: 'command_output.stdout.find("10.0.250.10") == -1'
```

### Ansible Loops

Ansible offers the loop , with_<lookup> , and until keywords to execute a task multiple times. 

### Ansible Roles

Make the work to re-use. Roles let you automatically load related vars, files, tasks, handlers, and other Ansible artifacts based on a known file structure. After you group your content in roles, you can easily reuse them and share them with other users.

```
---
- hosts: webservers
  roles:
    - role: '/path/to/my/roles/common'
```

Ansible galaxy is marketplace. [Ansible Galaxy](https://galaxy.ansible.com/) is a free site for finding, downloading, rating, and reviewing all kinds of community-developed Ansible roles and can be a great way to get a jumpstart on your automation projects.

`ansible-galaxy install <nameplaybook>`

just for linux - pywinrm is the way of using ansible on windows

### Ansible
