
2024-06-26 17:51

Source: #overthewire 

Tags: [[linux]]
### Leviathan Level 0->2, 3->4

whenever we get an [[ELF]] file then
1. use `cat file`, if the content of the file is less then this command is enough to make sense of the file
2. use `strings file`, if cat's output is messed up then use this
3. use `ltrace ./file`, to get a better understanding of the workings of the file 
### Level 2-3

- we are given a binary --> `printfile` . it uses` /bin/cat` to read contents from specific files 
- since it doesn't allow us to read the password of the next level we create a symbolic link to trick the file into giving us the password
- create a `tmp` directory create a file named "test 2".txt
- on giving "test 2".txt as input the binary interprets the input file as 2 different files i.e. test and 2.txt
- take advantage of this and create a symbolic link, linking "test" to the password file
```
ln -s /etc/leviathan_pass/leviathan3 /tmp/cryptocat/test
```
on running `./printfile "test 2".txt` we get the password in the 1st line of the output
### Level 5

we have a binary that checks for `file.log` in `tmp` folder ( we can't change the content of `tmp` folder)
```
ln -s /etc/leviathan_pass/leviathan6 /tmp/file.log
```
once the symbolic link is created we run the binary and get the password 
### Level 6

we have a binary that requires a 4 digit pass to unlock 
brute force the 4 digits using the following [[bash]] script 
```
for i in {0000..9999}
do
~/leviathan6 $i
done
```
on running this we get a shell running as level 7 user 
### References
https://www.youtube.com/watch?v=I6Ns7R7paJY&list=PLHUKi1UlEgOK1EgDbtL939pnou6inZ1WO&index=2