# Ansible

Ansible Control Manager - Ansible Targets

playbook.yml

keep watch - list or dictionary on yaml file.

Ansible has many modules.

Syntax

ansible is iterativ

ansible-playbook is decalarative method.

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

System
Commands
Files
Database
Cloud
Windows
More

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


"Ansible Lineinfile is **a module of Ansible that is used to modify the particular line in a file**. It is useful to add a new line, modify a line, replace a line, and remove an existing line in a file if it finds a specific text."


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

Make the work to re-use. Create mysql roles 
Ansible galaxy is marketplace
ansible-galaxy install <nameplaybook>

just for linux - pywinrm is the way of using ansible on windows