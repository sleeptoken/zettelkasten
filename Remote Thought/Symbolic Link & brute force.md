
2024-06-26 17:51

Source: #overthewire 

Tags: [[linux]] [[bash]]
### Leviathan level 0-2

whenever we get an ELF file then
1. use `cat file`, if the content of the file is less then this command is enough to make sense of the file
2. use `strings file`, if cat's output is messed up then use this
3. use `ltrace ./file`, to get a better understanding of the workings of the file 
### level 2-3

we are given a binary --> printfile . it use` /bin/cat` to read contents from specific files 
since it doesn't allow us to read the password of the next level we create a symbolic link to trick the file into giving us the password
create a tmp directory create a file named "test 2".txt
on giving "test 2".txt as input the binary interprets the input file as 2 different files i.e. test and 2
ln -s /etc/leviathan_pass/leviathan3 /

### References
https://www.youtube.com/watch?v=I6Ns7R7paJY&list=PLHUKi1UlEgOK1EgDbtL939pnou6inZ1WO&index=2