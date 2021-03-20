# LinuxNotes

- ### 1. FINDING STUFF
   -  [1.1 Find](https://github.com/pwnCmndr/LinuxNotes/blob/main/README.md#find) 
   -  [1.2 Whereis, locate & which](https://github.com/pwnCmndr/LinuxNotes/blob/main/README.md#whereis-locate--which)
   -  [1.3 Grep](https://github.com/pwnCmndr/LinuxNotes/blob/main/README.md#grep)
   -  [1.4 Wild Cards](https://github.com/pwnCmndr/LinuxNotes/blob/main/README.md#wild-cards)
   -  [1.5 Finding commands with man -k](https://github.com/pwnCmndr/LinuxNotes/blob/main/README.md#man--k)


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

