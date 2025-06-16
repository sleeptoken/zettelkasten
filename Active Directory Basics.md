
2025-06-16 15:47

Source: #tryhackme 

Tags: [[win]]

 - a **Windows domain** is a group of users and computers under the administration of a given business. 
 - The main idea behind a domain is to centralize the administration of common components of a Windows computer network in a single repository called **Active Directory (AD)**. 
 - The server that runs the Active Directory services is known as a **Domain Controller (DC)**.

Active Directory is also the component that allows your school/university to restrict you from accessing the control panel on your school/university machines. Policies will usually be deployed throughout the network so that you don't have administrative privileges over those computers.
## Active Directory 

﻿The core of any Windows Domain is the **Active Directory Domain Service (AD DS)**. This service acts as a catalogue that holds the information of all of the "objects" that exist on your network.
##### Users 

Users are one of the objects known as **security principals**, meaning that they can be authenticated by the domain and can be assigned privileges over **resources** like files or printers
- Users can be used to represent two types of entities:
	- **People**
	- **Services** you can also define users to be used by services like IIS or MSSQL. Every single service requires a user to run, but service users are different from regular users as they will only have the privileges needed to run their specific service.
##### Machines 

For every computer that joins the Active Directory domain, a machine object will be created. Machines are also considered "security principals" and are assigned an account just as any regular user. The machine accounts themselves are local administrators on the assigned computer

> Machine Account passwords are automatically rotated out and are generally comprised of 120 random characters.

Identifying machine accounts is relatively easy. The machine account name is the computer's name followed by a dollar sign. For example, a machine named `DC01` will have a machine account called `DC01$`.
##### Security Groups

Security groups are also considered security principals and, therefore, can have privileges over resources on the network.
- Groups can have both users and machines as members. If needed, groups can include other groups as well.
You can obtain the complete list of default security groups from the [Microsoft documentation](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups).

Go to "Active Directory Users and Computers" from the start menu, Here objects are organized in **Organizational Units (OUs)** which are container objects that allow you to classify users and machines. OUs are mainly used to define sets of users with similar policing requirements. user can only be a part of a single OU at a time.

| Security Group                                                                                                                                                                        | OUs                                                                                                                                                                                           |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Security Groups**, are used to **grant permissions over resources**. For example, you will use groups if you want to allow some users to access a shared folder or network printer. | **OUs** are handy for **applying policies** to users and computers, which include specific configurations that pertain to sets of users depending on their particular role in the enterprise. |
| A user can be a part of many groups, which is needed to grant access to multiple resources.                                                                                           | a user can only be a member of a single OU at a time, as it wouldn't make sense to try to apply two different sets of policies to a single user.                                              |

OUs are protected against accidental deletion. To delete the OU, we need to enable the **Advanced Features** in the View menu


### References
[TryHackMe | Active Directory Basics](https://tryhackme.com/room/winadbasics)