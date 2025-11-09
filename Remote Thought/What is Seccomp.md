
2025-11-05 23:37

Tags: [[linux]]

> The idea behind seccomp is to restrict the system calls that can be made from a process

The Linux kernel has a few hundred system calls, but most of them are not needed by any given process. If a process can be compromised and tricked into making other system calls, though, it may lead to a security vulnerability that could result in the compromise of the whole system. By restricting what system calls can be made, seccomp is a key component for building application sandboxes.
### References

[Welcome to LWN.net [LWN.net]](https://lwn.net/)