# Linux Fundamental Commands

`man` (manual)
`--help `(command option to find out options or help)
`whatis`
`whereis`
`which`
`history` (show command history) | `history -c `

"This is my personal study while learning Linux environment. 
I'm gonna share my experience for other people and to follow my notes."

file called always abckg.txt (you should change if you want to use it)

# Fundamental knowledge

`Input - Output -         Error
0                1                2`

### Highlighted topic:

**yourinput > addedyouroutput**
**yourinput >> addedyouroutput** (appended)
ls >  abckg.txt | cp abckg.txt abckgnew.txt && rm abckg.txt
`>` redirects output to a file, overwriting the file.
`>>` redirects output to a file appending the redirected output at the end.

### Piping:

Command-1 | Command-2 | …| Command-N

`find *.txt | less` (common usage with less in order to read easily.)
`double ||` : 1st command || 2nd command (if 1st be succeed,  no action. if not, do it 2nd command)
`&&` : 1st be executed and then it can go 2nd.*

`pwd` (show working directory)
`ls` (show everything)
`cd` (move directory)    
`which` cal
`mkdir` (make directory - WHAT)
`rmdir` (remove directory - WHAT )
`touch` (create file - WHAT )
`rm` (remove files or directory - WHAT)
`mv` (move or rename - WHAT WHERE)
`cp` (copy)
`>` (redirect)
`wc` (count)
`    ls | wc -w`
``|`` (pipe)
`find` (find files in directory)
`tail -f <abckg.txt>`
`head -n 5 <abckg.txt>`
`history` (show command history)
`\n` (newline)



`ps` (report current process) (ps -l, -u, -x) (`ps aux`)
`uname -a` (-r,-v,o,-n etc)
`lscpu`
`shutdown 13:20 or shutdown -h +2`
`top` (windows task manager :) 
`top -c` or `top 10 -d 10`  or `top -p 11` or `top -i`
`htop` (alternative top)
`kill` (terminate to process)
`du` (disk usage) (du -sh)
`df` (free space)
`free -m` (free mem)

`service --status-all`




### User

`su` (command with chosen user - WHO) #su kemal 
`passwd <username>`
`adduser <username>`
`sudo group add <groupname>`
`sudo usermod -G <groupname> <username>`

etc/passwd
etc/shadow
etc/group `tail -5 /etc/group`

d: directory
-: standart
r: read
w: write
x: execute
chmod +x abckg.txt
chmod 666 abckg.txt (rw)

### Network

etc/resolv.conf
etc/services
etc/hosts.allow
etc/hosts.deny
`install openssh-server`


`ifconfig`


`wget <link>`


`curl - o <link>`


`whois <domain>`


`traceroute 1.1.1.1`


`netstat -l` or `netstat -an` or `netstat -t` or `netstat -u`

`nano` (editor ^ = control or cmd)


`tar -cvf abckg.tar abckg.txt` (compress)


`tar -xvf abckg.tar abckg.txt` (extract)


`tar -cvzf abckg.tar.gz abckgfile1 abckgfile2` (one command for gzip)


`gzip -9 abckg.txt`


`bizp2 abckg.txt`


`bunzip abckg.txt`


`zip -r abcgk.zip abckg.txt abc.zip -P 123`

### bash scripting:#!/bin/bash

find apt packages:
`apt update `


`apt upgrade`


`apt-cache search docx` (find docx related packages)


`apt-cache show docx2txt` 


apt packages are **located cd /var/lib/appt/lists**
repos are **located sudo nano /etc/apt/sources.list**

uninstall:
`sudo apt-get purge abckg`
`sudo apt-get autoremove abckg `
