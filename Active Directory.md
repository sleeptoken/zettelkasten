
2024-12-31 13:22

Source: #AOC24 

Tags: [[win]] 

**Active Directory** (AD) is, therefore, a Directory Service at the heart of most enterprise networks that stores information about objects in a network. The associated objects can include:
- **Users** - accounts representing people or services
- **Groups** - Collections of users or other objects, often with specific permissions
- **Computers** - Machines that belong to the domain governed by AD policies
- **Printers** and other **resources** - Network-accessible devices

The building blocks of an AD architecture include:

- **Domains**:
	- Logical groupings of network resources such as users, computers, and services. They serve as the main boundary for AD administration and can be identified by their **D****omain Component and Domain Controller** name. Everything inside a domain is subject to the same security policies and permissions.
- **Organizational Units (OUs)**: 
	- OUs are containers within a domain that help group objects based on departments, locations or functions for easier management. Administrators can apply Group Policy settings to specific OUs, allowing more granular control of security settings or access permissions.
- **Forest**: 
	- A collection of one or more domains that share a standard schema, configuration, and global catalogue. The forest is the top-level container in AD.
- **Trust Relationships**: 
	- Domains within a forest (and across forests) can establish trust relationships that allow users in one domain to access resources in another, subject to permission.

Combining all these components allows us to establish the **Distinguished Name (DN)** that an object belongs to within the AD

### Core Active Directory Components

- **Domain Controllers (DCs):**
	- Domain Controllers are the servers that host Active Directory services. They store the AD database and handle authentication and authorization requests, such as logging in users or verifying access to resources. Multiple DCs can exist within a domain for redundancy. When changes are made to AD (such as adding users or updating passwords), these changes are replicated across all DCs, ensuring that the directory remains consistent.
- **Global Catalog:** 
	- The Global Catalog (GC) is a searchable database within AD that contains a subset of information from all objects in the directory. This allows users and services to locate objects in any domain in the forest, even if those objects reside in different domains.
- **[[LDAP]] (Lightweight Directory Access Protocol):**
	- AD uses this protocol to query and modify the directory. The protocol allows for fast searching and retrieving of information about objects such as users, computers, and groups.
- **Kerberos Authentication:** 
	- The default authentication protocol used by AD provides secure authentication by using tickets rather than passwords.

**Group Policy**

One of Active Directory's most powerful features is **Group Policy**, which allows administrators to enforce policies across the domain. Group Policies can be applied to users and computers to enforce password policies, software deployment, firewall settings, and more.

**Group Policy Objects (GPOs)** are the containers that hold these policies. A GPO can be linked to the entire domain, an OU, or a site, giving the flexibility in applying policies.x








### References
Day 15