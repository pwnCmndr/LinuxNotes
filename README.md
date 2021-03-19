# LinuxNotes

- ### 1. FINDING STUFF
   - #### [1.1 Find](https://github.com/pwnCmndr/LinuxNotes/blob/main/README.md#find) 
   - #### 1.2 Whereis, locate & which


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
