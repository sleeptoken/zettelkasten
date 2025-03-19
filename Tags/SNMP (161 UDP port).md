
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
1. Read Community String 
	- The configuration of the device or system can be viewed with the help of this password.  
	- These strings are public. 
2. Read/Write Community String 
	- The device configuration can be changed or edited using this password. 
	- These strings are private.

When administrators leave the community strings at the default setting, attackers can use these default community strings (passwords) for changing or viewing the configuration of the device or system. Attackers enumerate SNMP to extract information about network resources such as hosts, routers, devices, and shares as well as network information such as ARP tables, routing tables, device-specific information, and traffic statistics.
## Working of SNMP

The communication process between an SNMP manager and SNMP agent is as follows. 

1. Initialization (Start-up)
	When a network device boots up, the SNMP agent on the device initializes its configuration and prepares to communicate with the SNMP manager by listening on the designated port (usually UDP port 161). 

2. Discovery (Manager Discovers Agents)
	The SNMP manager discovers SNMP-enabled devices on the network by sending a request to the broadcast address or specific IP addresses where agents are known to reside. 

3. Information Exchange 
	The communication between an SNMP manager and agent involves several types of operations, primarily using SNMP messages called Protocol Data Units (PDUs). The key operations include: 
	
	a. Get Request 
		The SNMP manager sends a Get Request to an SNMP agent to retrieve the value of a specific variable, such as the status of a router interface or the bandwidth usage on a network link. 
	b. GetNext Request 
		This request is used to fetch the next variable in the MIB (Management Information Base) tree. It allows the manager to query a sequence of variables without knowing their exact names. 
	c. Set Request 
		The SNMP manager uses Set Requests to modify the value of a variable in the agent's MIB, effectively changing the configuration or behavior of the network device. 
	d. GetBulk Request 
		Introduced in SNMPv2, this operation allows the retrieval of large volumes of data with a single request, improving efficiency over multiple GetNext Requests. 
	e. Response 
		After receiving a Get, GetNext, Set, or GetBulk request, the SNMP agent processes the request, performs the necessary actions, and sends back a Response PDU containing the requested values or an acknowledgment of the action taken.
	f. Inform Request 
		An SNMP agent uses Inform Requests to send unsolicited information to the SNMP manager, usually about significant events or errors. This mechanism is used for manager-to-manager communication as well. 
	g. Trap 
		Traps are unsolicited messages sent from an SNMP agent to the manager to alert it of significant events or changes in the network, such as a device reboot or a link failure. SNMPv3 introduced the concept of Notifications, which encompasses both Traps and Informs, adding authentication and encryption. 

4. Monitoring and Management 
	The SNMP manager uses the information collected from SNMP agents to monitor network performance, detect and diagnose issues, and configure network devices remotely. This ongoing process involves regular polling (sending Get Requests) and listening for Traps or Inform Requests from agents. 
### Management Information Base (MIB) 

MIB is a virtual database containing a formal description of all the network objects that SNMP manages. It is a collection of hierarchically organized information. It provides a standard representation of the SNMP agent's information and storage. MIB elements are recognized using object identifiers (OIDs). An OID is the numeric name given to an object and begins with the root of the MIB tree. The OID can uniquely identify the object in the MIB hierarchy. 
MIB-managed objects include scalar objects, which define a single object instance, and tabular objects, which define a group of related object instances. OIDs include the object's type (such as counter, string, or address), access level (such as read or read/write), size restrictions, and range information. The SNMP manager converts the OIDs into a human-readable display using the MIB as a codebook. 
A user can access the contents of the MIB by using a web browser either by entering the IP address and Lseries.mib or by entering the DNS library name and Lseries.mib. For example, http://IP.Address/Lseries.mib or http://library_name/Lseries.mib.Microsoft provides the list of MIBs that are installed with the SNMP service in the Windows resource kit. The major MIBs are as follows: 
DHCP.MIB: Monitors network traffic between DHCP servers and remote hosts. 
HOSTMIB.MIB: Monitors and manages host resources 
LNMIB2.MIB: Contains object types for workstation and server services 
MIB_II.MIB: Manages TCP/IP-based Internet using a simple architecture and system 
WINS.MIB: For the Windows Internet Name Service (WINS) 
.


## Enumeration 






### References
