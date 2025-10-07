Attack techniques ka idea ho (ideally attacker side pe hath dala ho)
Linux internals ka idea ho
Detection ka idea ho.

For example, agar puchha ki file integrity kaise monitor karenge to fir answer me 2-3 option aa sakte hain
	- inotify ya fanotify use kar sakte hain 
	- ya syscall intercept kar sakte hain. open, openat etc.
- obviously agla sawal inke pros and cons pe hoga, inke thode se internals pe hoga
- inse implement kiya to evasion kaise hoga
- Jo evasion ka method diya, ab uska detection kaise hoga
Detection <—> Evasion

The loop goes on until candidate gives up.

Student was giving payloads for XSS and interviewer was adding defenses. We kept doing this until student gave up

> [!NOTE]
> instead of directly first jumping over tools, do a manual hands on, create own approach and then when u feel okay, then jump over to tools.

Interview mei kya puchenge 
- detection me RCE puchhenge. 
- Heap overflow puchhenge.
- Type confusion, 
- integer overflows/underflows, 
- TOCTTOU bugs
- kernel ki vulns
- memory alignment related issues

> Learning a system very deeply also gives you power to destroy it completely.

### web 

Candidate ko puchho innerHTML mai script context se XSS kyu nahi karte

What is DOM? Why do you need DOM? Kaha se aaya yeh? Parser kya hai?

BFSI mei ye vuln milega mostly 
- `xp_cmdshell` in MSSQL pata hai, But very rare case mai hota hai
- Mostly IDOR milega tumko

> XSS ko deeply samjhna hai toh browser behaviour samjho

### Cloud

Cloud hai. Tagged VLAN puchho logo ko kabhi
Bina VLAN tagging ke cloud me network separation kaise karoge?

namespace, cgroups and SECCOMP ke misconfig se 80% container breakout hote hain
janta kya karti hai? Unke liye har breakout ka technique ek naya magic hai
Container security is subset of linux internals.

> https://lwn.net/Kernel/Index/

> [!NOTE]
> Developers optimization pe dhyaan dete hai. And Optimization and Security are inversely proportional in some contexts


