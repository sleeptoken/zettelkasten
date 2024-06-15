
2024-06-15 11:21

Source: #forensics #tryhackme 

Tags: [[win]]

EventViewer is the best source to go to in order to see **user login history**. User logins are included in EventViewer under Windows Logs > Security

`$ net user` The Net User command is a **Windows command-line utility that allows you to manage Windows server local user accounts or on a remote computer

**What IP does the system connect to when it starts?**

The best way to see such events is to go to the Registry Editor (“regedit”). Once there, go to: HKEY_LOCAL_MACHINE > SOFTWARE > Microsoft > Windows > CurrentVersion > Run.

Why this registry key? In order to see what IP the system connects to, we need to see what the system does when it starts. The instructions for start-up are contained, inter alia, in this registry key.

**What was the attackers external control and command servers IP?**

In general, an attacker will oftentimes add the C2 server IP address to the hosts file. So open File Explorer > This PC > Local Disk (C:) > Windows > System32 > drivers > etc

**What was the extension name of the shell uploaded via the servers website?**

since it's a windows machine it is highly likely that the web server in use is IIS. According to [Stackify](https://stackify.com/what-is-inetpub/), inetpub “is the folder on a computer that is the default folder for Microsoft Internet Information Services (IIS). check `C: drive: inetpub.` The website content and web apps are stored in the inetpub folder — which keeps it organized and secure.”

### References
[TryHackMe | Investigating Windows](https://tryhackme.com/r/room/investigatingwindows)

walkthrough - 
[TryHackMe: Investigating Windows, Part 1 – HakstheHax](https://haksthehax.com/2021/05/25/tryhackme-investigating-windows-part-1/)