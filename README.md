# LinuxNotes
Basic Linux commands &amp; usage
# 1. FINDING STUFF

## Find


When using find without su "permission Denied"
`$ find / -type f -name firefox`
with sudo goes in deep within every account associated directory,
`$ sudo find / -type f -name firefox`
as 

1. search started form "/" but not limited to
2. type of simple file is mentioned by "f" 
3. name of the file firefox


The find command started at the top of the filesystem (/ ), went through every directory
looking for firefox in the filename, and then listed all instances found.


## Speeding up Find command with Specific Directory

![ca74379f104dd7812d1f8fcf4349c045.png](:/25eaf2a93abd43c7805db91d54495b5e)
```
$ find ~/ -type f -name firefox`
```
## Using wildcard with find

If the file firefox has an extension, such as
firefox.conf, the search will not find a match. We can remedy this limitation by using
wildcards, which enable us to match multiple characters. Wildcards come in a few
different forms: * . , ? and [ ] .
![d10cb898999bd71c6ebe2c401e690447.png](:/89b588560c1d4d6b80a0bc9415f4a374)

```
$ find ~/ -type f -name firefox*
```


![c61252577d3a8106e8050de88c10b7c3.png](:/d3f54be4beb34ea18fce8275ca0ab45e)
`$ find ~/ -type f -name firefox.*`
