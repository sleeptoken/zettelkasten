
2025-01-15 23:29

Confirmed Vulnerable Version: 6.3.1 - Linux application 
#### 1. [CVE-2023-47320: Broken Access Control Leading to Denial-of-Service](https://github.com/RhinoSecurityLabs/CVEs/tree/master/CVE-2023-47320)  

 An attacker with low privileges is able to execute the administrator-only function of putting the application in "Maintenance Mode" due to broken access control. This makes the application unavailable to all users.
##### Exploitation

After logging in as a low privileged user, go to this URL [http://localhost:8080/silverpeas/RjobStartPagePeas/jsp/ActivateMaintenance?allIntranet=1](http://localhost:8080/silverpeas/RjobStartPagePeas/jsp/ActivateMaintenance?allIntranet=1). This places the application in "Maintenance Mode" and makes it unavailable to all users.

##### 2. [CVE-2023-47321: Broken Access Control Allows Attacker to Access Portlet Deployer](https://github.com/RhinoSecurityLabs/CVEs/tree/master/CVE-2023-47321)

Silverpeas Core 6.3.1 is vulnerable to Incorrect Access Control via the "Portlet Deployet" which allows administrators to deploy .WAR [[portlets]].
###### Exploitation 

To exploit this vulnerability, an authenticated user needs to navigate directly to this URL: [http://localhost:8080/silverpeas/portletDeployer](http://localhost:8080/silverpeas/portletDeployer)

#### 3. [CVE-2023-47322: CSRF Leading to Privilege Escalation](https://github.com/RhinoSecurityLabs/CVEs/tree/master/CVE-2023-47322)

The "userModify" feature is vulnerable to Cross Site Request Forgery (CSRF) leading to privilege escalation. If an administrator goes to a malicious URL while being authenticated to the Silverpeas application, the CSRF will execute making the attacker and administrator user in the application.
##### Exploitation

To exploit this vulnerability, an attacker must host `CVE-2023-47322.html` on an attacker-controlled web server. When an authenticated administrator goes to the attacker's website, the CSRF will execute making the attacker an administrator.

#### 4. [CVE-2023-47323: Broken Access Control Allows Attacker to Read All Messages](https://github.com/RhinoSecurityLabs/CVEs/tree/master/CVE-2023-47323) 

The notification/messaging feature of Silverpeas Core 6.3.1 does not enforce access control on the ID parameter. This allows an attacker to read all messages sent between other users; including those sent only to administrators.
##### Exploitation

To exploit this vulnerability, an attacker can use a script or Burp Suite Intruder to view all messages by attacking the ID parameter in this URL: `http://localhost:8080/silverpeas/RSILVERMAIL/jsp/ReadMessage.jsp?ID=[messageID] `- the messages begin at "1" and increase in intervals of 1.

4. [CVE-2023-47324: Stored XSS in Messaging Feature](https://github.com/RhinoSecurityLabs/CVEs/tree/master/CVE-2023-47324)
5. [CVE-2023-47325: Broken Access Control on “Bin” Allows Modification by Attacker](https://github.com/RhinoSecurityLabs/CVEs/tree/master/CVE-2023-47325)
6. [CVE-2023-47326: CSRF Leading to Domain Creation](https://github.com/RhinoSecurityLabs/CVEs/tree/master/CVE-2023-47326)
7. [CVE-2023-47327: Broken Access Control Allows Attacker to Create Spaces](https://github.com/RhinoSecurityLabs/CVEs/tree/master/CVE-2023-47327)

### References
https://rhinosecuritylabs.com/research/silverpeas-file-read-cves/