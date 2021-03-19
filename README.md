# LinuxNotes

- ### 1. FINDING STUFF
   -  [1.1 Find](https://github.com/pwnCmndr/LinuxNotes/blob/main/README.md#find) 
   -  [1.2 Whereis, locate & which](https://github.com/pwnCmndr/LinuxNotes/blob/main/README.md#whereis-locate--which)
   -  [1.3 Grep](https://github.com/pwnCmndr/LinuxNotes/blob/main/README.md#grep)
   -  [1.4 Wild Cards]


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

```
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
## Among the most widely used wildcards
is the asterisk `(* )`, which matches any character(s) of any length,
from none to an unlimited number of characters. A search for ` *at` , for
example, would find `cat, hat, what, and bat`.

```
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

