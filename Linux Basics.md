# Linux Fundamental Commands

`man` (manual)


`--help ` (command option to find out options or help)


`whatis`


`whereis`

`which`

`whoami`

`file filename` to learn which type of file

`history` (show command history) | `history -c ` bash_history



"This is my personal study while learning Linux environment. 
I'm gonna share my experience for other people and to follow my notes."

file called always abckg.txt (you should change if you want to use it)

# Fundamental knowledge

`Input -        Output -         Error`


`"0"                    "1"                   "2"`

### Must learn:   >     |     &

`**yourinput > addedyouroutput**`

`**yourinput >> addedyouroutput**` (appended)

`ls >  abckg.txt | cp abckg.txt abckgnew.txt && rm abckg.txt`

`>` redirects output to a file, overwriting the file.

`>>` redirects output to a file appending the redirected output at the end.

### Piping:

Command-1 | Command-2 | …| Command-N

`find *.txt | less` (common usage with less in order to read easily.)

`&` run in background

`;` run all commands same time

`&&` : 1st be executed and then it can go 2nd.

`double ||` : 1st command || 2nd command (if 1st be succeed,  no action. if not, do it 2nd command "or")

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

`ln -s` shortcut

`ln` backup

`scp file usr@host:hostfolder` 

`scp -r`  for directory copy

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

`Ps -ef`

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

`/proc/meminfo`

`/proc/cpuinfo`

`Init 6` = `reboot`

`service --status-all`

### User

`su` (command with chosen user - WHO) #su kemal 


`<username>`


`adduser <username>`

`id username`


`sudo group add <groupname>`


`sudo usermod -G <groupname> <username>`



`Service … status/start/stop/reload/restart`


etc/passwd

etc/shadow


etc/group `tail -5 /etc/group`

`Chmod ugo`

`Chmod u+x file 1`
`Chmod u+x,g-w`

Read 4 write 2 exc 1
`Chmod 777`

d: directory

-: standart

r: read

w: write

x: execute

chmod +x abckg.txt

chmod 666 abckg.txt (rw)

`Chkconfig` startup service

### Network

etc/resolv.conf

etc/services

etc/hosts.allow

etc/hosts.deny

Etc/ssh/sshd_config

Etc/https/conf/httpd


`install openssh-server`



`ifconfig`



`wget <link>`



`curl - o <link>`



`whois <domain>`



`traceroute 1.1.1.1`



`netstat -l` or `netstat -an` or `netstat -t` or `netstat -u`

`Netstat -tulpn`


`nano` (editor ^ = control or cmd)

`Sed/change/change2/` (find and replace)


`tar -cvf abckg.tar abckg.txt` (compress)



`tar -xvf abckg.tar abckg.txt` (extract)



`tar -cvzf abckg.tar.gz abckgfile1 abckgfile2` (one command for gzip)

`tar -cvf dir1.tar dir1`

`tar -xvf dir1.tar dir1`

`tar -xvzf dir1.tar.gz`


`gzip -9 abckg.txt`



`bizp2 abckg.txt`



`bunzip abckg.txt`



`zip -r abcgk.zip abckg.txt abc.zip -P 123`

**Crontab - MinHourDayMonthWeek **


### bash scripting:#!/bin/bash

find apt packages:

`apt update `



`apt upgrade`



`apt-cache search docx` (find docx related packages)



`apt-cache show docx2txt` 



apt packages are **located cd /var/lib/appt/lists**

repos are **located sudo nano /etc/apt/sources.list

**

uninstall:


`sudo apt-get purge abckg`


`sudo apt-get autoremove abckg `
