
2025-01-15 23:29

Confirmed Vulnerable Version: 6.3.1 - Linux application 

> All the CVEs found by Tyler Ramsbey were discovered by extensively reading the documentation of the application 

####  [CVE-2023-47324: Stored XSS in Messaging Feature](https://github.com/RhinoSecurityLabs/CVEs/tree/master/CVE-2023-47324)

The notification/messaging feature  is vulnerable to Stored Cross-Site Scripting (XSS). 

Silverpeas checks for Cross-Site Scripting by filtering out `<script>` tags in messages. This check can be bypassed by performing XSS without using `<script>` tags.

the following payload can be sent in a message to another user: `<img/src/onerror=prompt(1)>`. When the user opens it, it successfully executes Javascript in the user's browser.
### Granting Attacker Additional Privileges

By analyzing the administrative request to elevate permissions, we discovered the following payload will successfully execute Javascript that elevates the adversary’s permissions: 

```html
<html><body onload="document.forms[0].submit();"><form action="http://localhost:8080/silverpeas/RjobDomainPeas/jsp/userModify" method="GET"><input type="hidden" name="Iduser" value="[userID]" /><input type="hidden" name="userLastName" value="[userLastName]" /><input type="hidden" name="userAccessLevel" value="ADMINISTRATOR" /><input type="hidden" name="X-STKN" value="[userSTKNToken]" /></form></body></html>
```

There are three pieces of information related to our _own_ user we need for a viable attack: 

1. The ID of our user. 
2. The last name of our user. 
3. The X-STKN of our user (the X-STKN is the Silverpeas Core CSRF token). 

information can be retrieved by sending a message to ourselves and intercepting the POST request with Burp Suite

With this information and the previous payload, an adversary can send a message to the administrators with the Javascript in the “content” field of the request
### File Read Through New Administrative Access

we can abuse a feature called “Silvercrawler.” Silvercrawl allows administrators to host files for other users to access. The Silvercrawler runs as the “root” user on the backend Linux server, so it can view sensitive files such as `/etc/shadow`.

Once Silvercrawler is enabled, we can specify the Root directory. To perform a full file read and enumerate the entire system, we can set the root directory as “/”. After this is configured, By navigating to the new path we created, we now have full access to the underlying file system
## Other CVE's
#### 1. [CVE-2023-47320: Broken Access Control Leading to Denial-of-Service](https://github.com/RhinoSecurityLabs/CVEs/tree/master/CVE-2023-47320)  

After logging in as a low privileged user, go to this URL`[http://localhost:8080/silverpeas/RjobStartPagePeas/jsp/ActivateMaintenance?allIntranet=1` This places the application in "Maintenance Mode" and makes it unavailable to all users.

#### 2. [CVE-2023-47321: Broken Access Control Allows Attacker to Access Portlet Deployer](https://github.com/RhinoSecurityLabs/CVEs/tree/master/CVE-2023-47321)

Silverpeas Core 6.3.1 is vulnerable to Incorrect Access Control via the "Portlet Deployet" which allows administrators to deploy .WAR [[portlets]].  an authenticated user needs to navigate directly to this URL: [http://localhost:8080/silverpeas/portletDeployer](http://localhost:8080/silverpeas/portletDeployer)

#### 3. [CVE-2023-47322: CSRF Leading to Privilege Escalation](https://github.com/RhinoSecurityLabs/CVEs/tree/master/CVE-2023-47322)

The "userModify" feature is vulnerable to Cross Site Request Forgery (CSRF) leading to privilege escalation. If an administrator goes to a malicious URL while being authenticated to the Silverpeas application, the CSRF will execute making the attacker and administrator user in the application.

#### 4. [CVE-2023-47323: Broken Access Control Allows Attacker to Read All Messages](https://github.com/RhinoSecurityLabs/CVEs/tree/master/CVE-2023-47323) 

The notification/messaging feature does not enforce access control on the ID parameter. This allows an attacker to read all messages sent between other users; including those sent only to administrators. URL: `http://localhost:8080/silverpeas/RSILVERMAIL/jsp/ReadMessage.jsp?ID=[messageID] `- the messages begin at "1" and increase in intervals of 1. 

#### 5. [CVE-2023-47325: Broken Access Control on “Bin” Allows Modification by Attacker](https://github.com/RhinoSecurityLabs/CVEs/tree/master/CVE-2023-47325)

The administrative "Bin" feature is affected by broken access control. A user with low privileges (authenticated) is able to navigate directly to the bin URL - `http://localhost:8080/silverpeas/RjobStartPagePeas/jsp/ViewBin`, revealing all deleted spaces. The user can then restore or permanently delete the spaces.

#### 6. [CVE-2023-47326: CSRF Leading to Domain Creation](https://github.com/RhinoSecurityLabs/CVEs/tree/master/CVE-2023-47326)

Silverpeas Core is vulnerable to Cross Site Request Forgery (CSRF) via the Domain SQL Create function. If an attacker clicks a malicious URL while authenticated to Silverpeas Core, the CSRF payload will create additional domains for authentication.

#### 7. [CVE-2023-47327: Broken Access Control Allows Attacker to Create Spaces](https://github.com/RhinoSecurityLabs/CVEs/tree/master/CVE-2023-47327)

To exploit this vulnerability, an attacker with low privileges needs to navigate directly to this URL with their X-STKN token: `http://localhost:8080/silverpeas/RjobStartPagePeas/jsp/CreateSpace?X-STKN=[Users-STKN-Token]`. The attacker can then type in a name and description and click "Ok" and the space is successfully created.

### References
https://rhinosecuritylabs.com/research/silverpeas-file-read-cves/