
2024-06-20 19:42

Source: #bandit #overthewire 

Tags: [[Shell Escape]] [[linux]]

Bandit 32 -> 33

Linux has **Variables** namely 
1. local variables (valid in current shell)
2. shell variables (set up by shell) 
3. environment variables (valid systemwide). 

- These variables have their names in uppercase only. They are defined by writing `VAR_NAME=var_value` in the command line.
- To print all environment variables, you can use `printenv`.

we have access to UPPERCASE shell which interprets all commands in uppercase only
```
>> ls
sh: 1: LS: not found
```
positional parameters are used that means borne shell might be used 
```
nmap 10.10.10.10 -sV
$0   $1          $2
here $ is used to denote positional parameters
```
you can view local and global environment variables 
in order to interact with the borne shell we need to type the positional parameter of the borne shell command `$0`
```
>> $0
$ export SHELL=/bin/bash                -> sets default shell 
$ $SHELL
```




### References
[OverTheWire: Level Goal: Bandit Level 32 → Level 33](https://overthewire.org/wargames/bandit/bandit33.html)

walkthrough: 
[OverTheWire Bandit Level 32 -> 33 - Walkthrough - MayADevBe Blog](https://mayadevbe.me/posts/overthewire/bandit/level33/)

borne shell manual- 
https://www.in-ulm.de/~mascheck/bourne/v7/

how to use positional params- 
https://bash.cyberciti.biz/guide/How_to_use_positional_parameters