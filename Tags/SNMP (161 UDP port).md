
2024-12-24 18:13

- SNMP allows network administrators to manage network devices from a remote location.
SNMP is a application layer protocol that runs on UDP and maintains and manages routers, hubs and switches on an IP network 

SNMP employs 2 types of software components for communication :
- **The SNMP agent** is located on the networking device
- **SNMP management station** communicates w the agent 

The SNMP management station sends requests to the agent; after receiving the request, the agent replies. 
- Both requests and replies are configuration variables accessible by the agent software.
- SNMP management stations send requests to set values to some variables.
- Traps let the management station know if an abnormal event such as a reboot or an interface failure has occurred at the agent's side. 
SNMP contains the following two passwords for configuring and accessing the SNMP agent from the management station. 
. 
Read Community String 
15 
The configuration of the device or system can be viewed with the help of this password.  
• These strings are public. 
Read/Write Community String 
• The device configuration can be changed or edited using this password. 
• These strings are private. 
When administrators leave the community strings at the default setting, attackers can use these default community strings (passwords) for changing or viewing the configuration of the device or system. Attackers enumerate SNMP to extract information about network resources such as hosts, routers, devices, and shares as well as network information such as ARP tables, routing tables, device-specific information, and traffic statistics.
## Enumeration 






### References
