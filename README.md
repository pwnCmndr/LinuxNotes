# LinuxNotes

- 1. FINDING STUFF
     - [1.1 Find](#find) 
     - [1.2 Whereis, locate & which](#whereis-locate--which)
     - [1.3 Grep](#grep)
     - [1.4 Wild Cards](#wild-cards)
     - [1.5 Finding commands with man -k](#man--k)

-  2. TEXT MANIPULATION
      - [2.1 Head , tail & nl](#head-tail--nl)
      - [2.2 Sed](#sed)

- 3. NETWORK
     - [3.1 Ifconfig](#ifconfig)
     - [3.2 Dhclient](#dhclient)
     - [Changing DNS](#dhclient)
     - [Dig (Domain Info Groper)](#changing-dns)
     - [ssh](#ssh)
- 4. PERMISSIONS
     - [Chmod & chown](#chmod--chown)
     - [umask](#umask)
     - [SUID](suid)

#

### Find
When using find without su, "permission Denied"

`$ find / -type f -name firefox`

with sudo goes in deep within every account associated directory,

`$ sudo find / -type f -name firefox`

as 

1. search started form "/" but not limited to
2. type of simple file is mentioned by "f" 
3. name of the file "firefox"


The find command started at the top of the filesystem (/ ), went through every directory
looking for firefox in the filename, and then listed all instances found.


#### Speeding up Find command with Specific Directory

```
$ find ~/ -type f -name firefox`
```
#### Using wildcard with find

If the file firefox has an extension, such as
firefox.conf, the search will not find a match. We can remedy this limitation by using
wildcards, which enable us to match multiple characters. Wildcards come in a few
different forms: * . , ? and [ ] .

```
$ find ~/ -type f -name firefox*
```
#
### Whereis, locate & which

`whereis` can search binary files 

`whereis python3`

`locate` gives overwhelming results 
`updatedb` command updates locate results

`locate python3`

The `which` command is simply for finding path for a specific path variable of a binary in linux

`which python3`

#
### Grep

Very often when using the command line, you’ll want to search for a particular
keyword. For this, you can use the grep command as a filter to search for keywords.


`$ ps aux`  // shows all running processes , but to get a specific process we can use grep 
e.g.

`$ ps aux | grep firefox-bin` 

`$cat /etc/snort/snort.conf | grep output` // filtering out the lines containing `output` word

#
### Wild Cards

Using wildcards `?,[a,b],*` with search options 

```bash
mode@debian:~$ mkdir testfolder
mode@debian:~$ find ?der
find: ‘?der’: No such file or directory
mode@debian:~$ find ?folder
find: ‘?folder’: No such file or directory
mode@debian:~$ find ?estfolder
testfolder
mode@debian:~$ find t?estfolder
find: ‘t?estfolder’: No such file or directory
mode@debian:~$ find t[e,t]stfolder
testfolder
mode@debian:~$ find *folder
testfolder
```
search on a directory that has the files cat, hat, what,and bat. 
The ? wildcard is used to represent a single character, 
so any search for `?at` would find `hat, cat, and bat` but not what, because at in this filename is preceded by two letters. 
The `[ ]` wildcard is used to match the characters that appear inside the square brackets. 
For example, 
Any search for `[c,b]at` would match `cat and bat` but not hat or what. 

#### Among the most widely used wildcards
is the asterisk `(* )`, which matches any character(s) of any length,
from none to an unlimited number of characters. A search for ` *at` , for
example, would find `cat, hat, what, and bat`.

```bash
mode@debian:~/testfolder$ touch cat hat bat what
mode@debian:~/testfolder$ ls [a,b,c]at
bat  cat
mode@debian:~/testfolder$ find ?at
bat
cat
hat
mode@debian:~/testfolder$ find *at
bat
cat
hat
what
mode@debian:~/testfolder$ 
```
#
### man -k

To find information in man pages, you can search the `mandb` database by using
`apropos or man -k`. If the database is current, getting access to the information you need is easy.

When using `man -k` to find specific information from the man pages, you’ll
sometimes get a load of information. If that happens, it might help to filter down the results a bit by using the `grep` command.

Man pages are categorized in different sections. The most relevant sections for system administrators are as follows:
```
 1: Executable programs or shell commands

 5: File formats and conventions

 8: System administration commands
```

**Updating mandb**
 `#mandb`

 ```bash
 mode@debian:~/testfolder$ man -k admin
brctl (8)            - ethernet bridge administration
gpasswd (1)          - administer /etc/group and /etc/gshadow
intro (8)            - introduction to administration and privileged commands
ip6tables (8)        - administration tool for IPv4/IPv6 packet filtering and...
iptables (8)         - administration tool for IPv4/IPv6 packet filtering and...
lpadmin (8)          - configure cups printers and classes
neo4j-admin (1)      - Neo4j administration
net (8)              - Tool for administration of Samba and remote CIFS servers.
netkey-tool (1)      - administrative utility for Netkey E4 cards
samba-tool (8)       - Main Samba administration tool.
virt-admin (1)       - daemon administration interface
 ```

#

### head, tail & nl

```bash
mode@debian:/$ ls -la
total 76
drwxr-xr-x    1 root root  258 Mar 20 11:58 .
drwxr-xr-x    1 root root  258 Mar 20 11:58 ..
...........
drwxr-xr-x    1 root root   98 Oct 22 09:49 var
lrwxrwxrwx    1 root root   28 Feb  4 14:05 vmlinuz -> boot/vmlinuz-4.19.0-14-amd64
lrwxrwxrwx    1 root root   28 Feb  4 14:05 vmlinuz.old -> boot/vmlinuz-4.19.0-13-amd64

```

`#head -5 file` 

> take 5 lines from the top  

```bash
mode@debian:/$ ls -la | head -5 
total 76
drwxr-xr-x    1 root root  258 Mar 20 11:58 .
drwxr-xr-x    1 root root  258 Mar 20 11:58 ..
lrwxrwxrwx    1 root root    7 Oct 19 22:53 bin -> usr/bin
drwxr-xr-x    1 root root  514 Feb 26 13:14 boot

```

`#tail -5 file` 

> take last 5 lines from the bottom

```sh
mode@debian:/$ ls -la > /home/mode/rootls
mode@debian:/$ cd /home/mode
mode@debian:~$ tail -5 rootls
drwxrwxrwt    1 root root 3740 Mar 20 15:54 tmp
drwxr-xr-x    1 root root  170 Jan 14 17:24 usr
drwxr-xr-x    1 root root   98 Oct 22 09:49 var
lrwxrwxrwx    1 root root   28 Feb  4 14:05 vmlinuz -> boot/vmlinuz-4.19.0-14-amd64
lrwxrwxrwx    1 root root   28 Feb  4 14:05 vmlinuz.old -> boot/vmlinuz-4.19.0-13-amd64
```

`#nl file` 

> number the lines of the file

```sh
mode@debian:~$ nl rootls
     1	total 76
     2	drwxr-xr-x    1 root root  258 Mar 20 11:58 .
     3	drwxr-xr-x    1 root root  258 Mar 20 11:58 ..
     .......
    29	lrwxrwxrwx    1 root root   28 Feb  4 14:05 vmlinuz -> boot/vmlinuz-4.19.0-14-amd64
    30	lrwxrwxrwx    1 root root   28 Feb  4 14:05 vmlinuz.old -> boot/vmlinuz-4.19.0-13-amd64

```

**hacker challenge display the five lines immediately before a line that says `"drwx------    1 root root  724 Mar 20 13:42 root"` using the commands you just learned**

```sh
mode@debian:~$ nl rootls | grep 724
    20	drwx------    1 root root  724 Mar 20 13:42 root
mode@debian:~$ nl rootls
     1	total 76
     2	drwxr-xr-x    1 root root  258 Mar 20 11:58 .
     3	drwxr-xr-x    1 root root  258 Mar 20 11:58 ..
     ......
    15	lrwxrwxrwx    1 root root   10 Oct 19 22:53 libx32 -> usr/libx32
    16	drwxr-xr-x    1 root root   30 Dec 23 17:25 media
    17	drwxr-xr-x    1 root root    0 Oct 19 22:53 mnt
    18	drwxr-xr-x    1 root root   92 Jan 25 14:27 opt
    19	dr-xr-xr-x  425 root root    0 Mar 19 20:04 proc
    20	drwx------    1 root root  724 Mar 20 13:42 root
    .......
    29	lrwxrwxrwx    1 root root   28 Feb  4 14:05 vmlinuz -> boot/vmlinuz-4.19.0-14-amd64
    30	lrwxrwxrwx    1 root root   28 Feb  4 14:05 vmlinuz.old -> boot/vmlinuz-4.19.0-13-amd64
mode@debian:~$ tail -16 rootls | head -5 
lrwxrwxrwx    1 root root   10 Oct 19 22:53 libx32 -> usr/libx32
drwxr-xr-x    1 root root   30 Dec 23 17:25 media
drwxr-xr-x    1 root root    0 Oct 19 22:53 mnt
drwxr-xr-x    1 root root   92 Jan 25 14:27 opt
dr-xr-xr-x  425 root root    0 Mar 19 20:04 proc

```

Alternatively

```sh
mode@debian:~$ head -19 rootls | tail -5
lrwxrwxrwx    1 root root   10 Oct 19 22:53 libx32 -> usr/libx32
drwxr-xr-x    1 root root   30 Dec 23 17:25 media
drwxr-xr-x    1 root root    0 Oct 19 22:53 mnt
drwxr-xr-x    1 root root   92 Jan 25 14:27 opt
dr-xr-xr-x  425 root root    0 Mar 19 20:04 proc
```

#

### sed

The sed command lets you search for occurrences of a word or a text pattern and then perform some action on it. sed operates like the Find and Replace function in Windows.  

`$sed s/mysql/MySql/g file1 > file2`  

The `s` command performs the search: you first give the term you are searching for (`mysql`)  and then the term you want to replace it with (`MySQL`)
Separated by a slash (`/` ). 
The `g` command tells Linux that you want the replacement performed **globally**. 
Then the result is saved to a new file named snort2.conf.  

can also use the sed command to find and replace any specific occurrence of a word  
rather than all occurrences or just the first occurrence

### Selecting lines 1 to 4

`sed -n '1,4p' demo.txt `
### selecting multiple sections
`sed -n -e '1,4p' -e '5,8p' demo.txt`

### Skipping lines
`sed -n '1~2p' demo.txt`
```
// skip every 2nd line
```
### Search lines starting with 
`sed -n '/^And /p' demo.txt`
```
^ - start of the line
p - print
```
### Substitute case insensivity
`sed -n 's/day/week/gip' demo.txt`
```
i - insensivity
substitute day with week
```
`sed -n 's/[Dd]ay/week/gp' demo.txt`

### Substitute in section
`sed -n '1,4 s/  */ /gp' demo.txt`
```
mode@debian:~$ sed -n '1,4p' demo.txt 
Day   after day, day   after day,
We stuck, nor breath   nor motion;
As idle   as a painted  ship
Upon   a   painted        ocean.
```
```
mode@debian:~$ sed -n '1,4 s/  */ /gp' demo.txt 
Day after day, day after day,
We stuck, nor breath nor motion;
As idle as a painted ship
Upon a painted ocean.
```
// replacring multispacing with single space
// * - represents 0 or more of the preciding chars

### Simultaneous substitution
`sed -n -e 's/motion/flutter/gip' -e 's/ocean/gutter/gip' demo.txt`

same as above

`sed -n 's/motion/flutter/gip;s/ocean/gutter/gip' demo.txt`

### Conditional substitution
`sed -n '/after/ s/[Dd]ay/week/gp' demo.txt`
```
mode@debian:~$ sed -n '/after/ s/Day/week/gp' demo.txt
week   after week, week   after week,
```

#

### ifconfig

**Changing ip Addr**

```sh
root@kali:~# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST> mtu 1500
       inet 192.168.1.100 netmask 255.255.255.0 broadcast 192.162.1.255
       inet6 fe80::20c:29ff:fe**:**** prefixlen 64 scopeid 0x20<link>
```

```sh
root@kali:~# ifconfig eth0 192.168.1.109
root@kali:~# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST> mtu 1500
       inet 192.168.1.109 netmask 255.255.255.0 broadcast 192.162.1.255
       inet6 fe80::20c:29ff:fe**:**** prefixlen 64 scopeid 0x20<link>
```



**Changing Your Network Mask and Broadcast Address**

```sh
root@kali:~# ifconfig eth0 192.168.1.100 netmask 255.255.0.0 broadcast 192.168.1.210
root@kali:~# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST> mtu 1500
       inet 192.168.1.109 netmask 255.255.255.0 broadcast 192.162.1.255
       inet6 fe80::20c:29ff:fe**:**** prefixlen 64 scopeid 0x20<link>
       ether **:**:**:**:**:**:** txqueuelen 1000 (Ethernet)
       RX packets 1377306 bytes 2002046786 (1.8 GiB)
```

**Spoofing Your MAC Address**
Changing your MAC address to spoof a different MAC address is
almost trivial and neutralises those security measures. Thus, it’s a very useful technique
for bypassing network access controls.

```sh
$ifconfig eth0 down  
$ifconfig eth0 hw ether 00:11:22:33:44:55 // spoffed mac address  
$ifconfig eth0 up  
```

#

### dhclient

The DHCP server assigns IP addresses to all the systems on the subnet and keeps log files of which IP address is allocated to which machine at any one time. This makes it a great resource for forensic analysts to trace hackers with after an attack. For that reason, it’s useful to understand how the DHCP server works.

Usually, to connect to the internet from a LAN, you must have a DHCP­assigned IP.

Therefore, after setting a static IP address, you must return and get a new DHCP assigned IP address. To do this, you can always reboot your system, but I’ll show you how to retrieve a new DHCP without having to shut your system down and restart it.

To request an IP address from DHCP, simply call the DHCP server with the command dhclient followed by the interface you want the address assigned to. Different Linux distributions use different DHCP clients, but Kali is built on Debian, which uses dhclient.

`dhclient eth0`

try `ping` ing to make the process speed up.

#

### Changing DNS

In some cases, you may want to use another DNS server. To do so, you’ll edit a plaintext file named `/etc/resolv.conf` on the system.

```sh
root@kali:~# nano /etc/resolv.conf
```

```sh
GNU nano 4.9.2
nameserver 192.168.1.1
```

if `nameserver 8.8.8.8` then dns is changed from public to google

#

### dig

One of the most useful commands for the aspiring hacker is dig , which offers a way to gather DNS information about a target domain.

The stored DNS information can be a key piece of early reconnaissance to obtain before attacking. This information could include the IP address of the target’s nameserver (the server that translates the target’s name to an IP address), the target’s email server, and potentially any subdomains and IP addresses.

\#Dig HowTo [https://www.madboa.com/geek/dig/]

#### get the address(es) for yahoo.com

```
dig yahoo.com A +noall +answer
```

#### get a list of yahoo’s mail servers

```
dig yahoo.com MX +noall +answer
```

#### get a list of DNS servers authoritative for yahoo.com

```
dig yahoo.com NS +noall +answer
```

#### get all of the above

```
dig yahoo.com ANY +noall +answer
```

**More obscurely, for the present anyway, you can also poll for a host’s IPv6 address using the AAAA option.**

```
dig www.isc.org AAAA +short
```

**If the domain you want to query allows DNS transfers, you can get those, too. The reality of life on the Internet, however, is that very few domains allow unrestricted transfers these days.**

```
dig yourdomain.com AXFR
```

#

### ssh

## Logging in with SSH key stored file

```
ssh root@192.168.x.x -i /path/to/id_rsa.txt
```

> Most of the servers do not permit root login. Hence, you can edit the `sshd config` file and change the `root login to yes` and `restart the SSH service` on the target.

Try to backdoor only a single user such as the root since, **most of the folks won’t log in through the root as default configurations prohibit it.**

#

### chmod & chown

The idea is to put people with similar needs into a group that is granted relevant permissions; then each member of the group
inherits the group permissions. This is primarily for the ease of administering permissions and, thus, security.
The root user is part of the root group by default. Each new user on the system must be added to a group in order to inherit the permissions of that group.

```sh
root@kali:~# ls -l /tmp/bobsfile
-rw-r--r-- 1 root root 23 May 19 21:05 /tmp/bobsfile
```

#### Changing Ownership

```sh
root@kali:~# chown bob /tmp/bobsfile
root@kali:~# ls -l /tmp/bobsfile
-rw-r--r-- 1 bob root 23 May 19 21:05 /tmp/bobsfile
```



The root group needs access to the hacking tools, whereas the security folk only need access to defensive tools such as an intrusion detection system (IDS). Let’s say the root group downloads and installs a program named newIDS; the root group will need to change the ownership to the security group so the security group can use it at will. To do so, the root group would simply enter the following command:

```
kali > chgrp security newIDS
```

This command passes the `security group`  ownership of `newIDS` .

```sh
root@kali:~# ls -l /tmp/bobsfile
-rw- r-- r-- 1 bob root 23 May 19 21:05 /tmp/bobsfile
 ___ ___ ___
 |   |   |
 |   |   Others
 |  Group
Owner 
```

## chmod to change permissions

```sh
root@kali:~# ls -l /tmp/bobsfile
-rw-r--r-- 1 bob root 23 May 19 21:05 /tmp/bobsfile

root@kali:~# chmod 777 /tmp/bobsfile
root@kali:~# ls -l /tmp/bobsfile
-rwxrwxrwx 1 bob root 23 May 19 21:05 /tmp/bobsfile

root@kali:~# chmod 776 /tmp/bobsfile
root@kali:~# ls -l /tmp/bobsfile
-rwxrwxrw- 1 bob root 23 May 19 21:05 /tmp/bobsfile

root@kali:~# chmod 644 /tmp/bobsfile
root@kali:~# ls -l /tmp/bobsfile
-rw-r--r-- 1 bob root 23 May 19 21:05 /tmp/bobsfile
```



## Granting Temporary Root Permissions with SUID

a file that allows users to change their password would need access to the `/etc/shadow` file—the
file that holds the users’ passwords in Linux—which requires root user privileges in
order to execute. In such a case, you can temporarily grant the owner’s privileges to
execute the file by setting the SUID bit on the program.
Basically, the SUID bit says that any user can execute the file with the permissions of the
owner but those permissions don’t extend beyond the use of that file.
To set the SUID bit, enter a 4 before the regular permissions, so a file with a new resulting
permission of 644 is represented as 4644 when the SUID bit is set.

```
#chmod 4644 filename
```

## Granting the Root User’s Group Permissions SGID

**SGID also grants temporary elevated permissions, but it grants the permissions of the file**
owner’s group, rather than of the file’s owner. This means that, with an SGID bit set,
someone without execute permission can execute a file if the owner belongs to the
group that has permission to execute that file.
The SGID bit works slightly differently when applied to a directory: when the bit is set on
a directory, ownership of new files created in that directory goes to the directory
creator’s group, rather than the file creator’s group. This is very useful when a directory
is shared by multiple users. All users in that group can execute the file(s), not just a
single user.

The SGID bit is represented as 2 before the regular permissions, so a new file with the
resulting permissions 644 would be represented as 2644 when the SGID bit is set. Again,
you would use the chmod command for this—for example,

```
# chmod2644 filename
```

#

### umask

SETTING MORE SECURE DEFAULT PERMISSIONS WITH MASKS

You can change the default permissions allocated to files and directories created by each user with the umask (or unmask) method. The umask method represents the permissions you want to remove from the base permissions on a file or directory to make them more secure.

The umask is a three­digit decimal number corresponding to the three permissions digits, but the umask number is subtracted from the permissions number to give the new permissions status. This means that when a new file or directory is created, its permissions are set to the default value minus the value in umask .

```
New Files         New directories
  666                 777                   Linux base permissions
 -022                -022                   umask
  644                 755                   Resulting permissions
```

In Kali, as with most Debian systems, the umask is preconfigured to 022, meaning the Kali default is 644 for files and 755 for directories.

The umask value is not universal to all users on the system. Each user can set a personal default umask value for the files and directories in their personal .profile file.

To see the current value when logged on as the user, simply enter the command `umask`.

```sh
root@kali:~# umask
0022
```

With Default umask set to 0022

Changing umask value to 0077

```sh
root@kali:~# umask 0077
root@kali:~# umask
0077
root@kali:~# touch file && ls -l file
-rw------- 1 root root 0 May 20 00:57 file
```

Just created file lacks in permission

#

### SUID

As a hacker, these special permissions can be used to exploit Linux systems through privilege escalation, whereby a regular user gains root or sysadmin privileges and the associated permissions. With root privileges, you can do anything on the system. One way to do this is to exploit the SUID bit. A system administrator or software developer might set the SUID bit on a program to allow that program access to files with root privileges. For instance, scripts that need to change passwords often have the SUID bit set. You, the hacker, can use that permission to gain temporary root privileges and do something malicious, such as get access to the passwords at /etc/shadow.

we want to find files anywhere on the file system, for the root user or other sysadmin, with the permissions 4000 . To do this, we can use the following find command: 

```sh
# find / -user root -perm -4000
/usr/bin/chfn
/usr/bin/chsh
......
```

1. find every file under `/`
2. which are owned by `root`
3. which have `SUID` permission bit set

```sh
/usr/bin# ls -l | grep rwsr
```

To Identify SUID pattern is rwsr

the first set of permissions—for the owner—has an s in place of the x . This is how Linux represents that the SUID bit is set.

This means that anyone who runs the sudo file has the privileges of the root user, which can be a security concern for the sysadmin and a potential attack vector for the hacker. For instance, some applications need to access the /etc/shadow file to successfully complete their tasks. If the attacker can gain control of that application, they can use that application’s access to the passwords on a Linux system.

```sh
/usr/bin# ls -l | grep rwxr-sr
```

To Identify SUID pattern is  rwxr-sr-x  valid after rwxr-s                      owner|group|users

