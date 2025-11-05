
2025-11-03 18:24

Source: #paloalto 

Tags: [[Defence]]
## Automation components

### Marketplace
is the central hub for managing content packs, which enhance security operations with content such as playbooks, integrations, scripts, and more. It also helps optimize case and asset management.

>  Content Pack Features 

Cortex provides the full potential of automation through a rich set of features that empower organizations to fortify their security posture and streamline case management.
##### Integrations

Integrations connect third-party security and alert management platforms to Cortex. This connection enables these platforms to trigger events, which are then transformed into issues within Cortex, initiating automated security processes.
##### Playbooks \& Scripts

Playbooks efficiently automate case handling, minimizing response times and ensuring consistency, while scripts provide customized automation tailored to organizational requirements.

- Playbooks are triggered by ingested issues, enabling the definition of a sequence of actions for specific case types. They offer adaptability and resource allocation optimization.
- Scripts are composed of a series of commands that can be employed in playbook tasks and when running commands in the issue War Room.
##### Issue Types and Fields

Issue types are essential for categorizing and prioritizing issues within Cortex, serve as the first step in classifying issues, determining the most suitable response and actions for each issue. Each issue type corresponds to a specific set of issue fields, ensuring the association of relevant information with each issue

Issue fields provide detailed information about each issue, capturing specific data relevant to the corresponding issue type.
##### Rules

**Correlation Rules** 
analyze multiple events from various sources using the XQL-based engine within Cortex. They are scheduled and utilize the XQL engine, allowing for rule-based threat detection.

**Data Model Rules** 
play a pivotal role in data normalization and enrichment. They allow the creation of a standardized schema with predefined data types and the mapping of third-party data to this schema. (making data from various sources easily consumable and consistent in format.)

**Parsing Rules**
Enable the preprocessing of incoming data, allowing for the removal of unnecessary information, data quality improvement, and storage optimization.
##### Dashboards and Reports

Dashboards
consolidate the view of security operations, enabling prompt decision-making and enhancing situational awareness. w  Customizable widgets

Reports
facilitate data-driven decision-making and the communication of security insights to relevant parties.

### Playbook
orchestrating a sequence of tasks, conditions, automations, commands, and loops to streamline and enhance investigation and response processes. *reduces investigation time*.

Playbooks come into play during the investigation and response phase of the issue lifecycle. Serve as the core automation engine within the cortex system.

Playbooks and case lifecycle

1. Investigation and response
2. Design use cases (The use case is the description of what it is that the SIEM should be alerting on)
3. Create and update playbook lifecycle
4. Issue Handling
5. Define Logical flow
6. Remediations criteria
7. Continuous improvement

**Types of playbook**

Manual tasks
Conditional tasks - validate conditions based on parameters
Communication tasks - facilitate interaction w others
Automation tasks - automatically remediate issues by engaging third-party integrations, ticketing systems, or file detonation using a sandbox









### Filters and Transformers
allowing users to extract, manipulate, and present data in JSON format

### Scripts 
Scripts within Cortex serve as potent tools for executing specific actions through commands. A script is attached by default, making it uneditable. To modify a script, users can detach it or create a duplicate. 



### Work Plan 

Work plans are a part of issues, and contains the playbooks that are slated to be run for that issue. Playbooks are used to consistently perform and document security monitoring, orchestration, and response activities.

**Work plan options**

- Change the playbook
- Follow a Playbook - see running in real time
- Run Again
- zoom
- Export Playbook - in png
#### Playbook management

Sub-Playbook Loops
involve the creation of loops within a parent playbook using sub-playbooks. The parent playbook can be configured to exit the loop based on conditions like "For Each Input" or "Built-in." This feature is instrumental for iterating over a set of tasks multiple times, enhancing automation capabilities.

Playbook polling
is employed when waiting for a process to conclude on a remote host. The GenericPolling playbook periodically polls the status of a process, ensuring the completion of the playbook once the remote action concludes. This feature is especially useful for tasks that require waiting for external processes.

Issue Field mgmt
Creating issue fields in a playbook is an iterative process. The set issue script in a playbook task plays a crucial role in setting and updating issue fields during investigations, providing a dynamic approach based on evolving needs and available information.

Playbook Debugger
Debugger features include setting breakpoints, overriding inputs and outputs, skipping tasks, and viewing context data and indicators.

### Playbook tasks

Depending on the task type, playbooks feature inputs and outputs crucial for task execution. Sub-playbooks, categorized as parent or child playbooks, provide modularity and reusability in playbook development.
#### Metadata

Understanding playbook metadata helps in troubleshooting system performance issues, users can analyze playbook task i/p & o/p, storage usage and task types.
- Navigate to the Case War Room on the Cases page and run the following command:
```
!getInvPlaybookMetaData caseId=<case ID> minSize=<size of the data you want to return in KB. Default is 10>
```
#### Task Execution and Management

Playbooks are executed when cases are ingested, automatically running through their predefined workflows.
Users can view and update playbooks associated with cases or issues in the work plan section.
#### Auth for comms tasks

Organizations can configure SSO by setting up dedicated groups of external users in their identity provider (IdP) such as Okta. Parameters such as Single Sign-On URL, Audience URI (Unified Resource Identifier), and IdP SSO URL are specified to establish a secure authentication process.

#### Adding an Ad-Hoc Task

Within the work plan, you can create tasks for a specific iteration of a playbook. The task type can be an automation or another playbook. For example, within a manual task, you might need to enrich some data when running an investigation playbook.

### Playbook task types

You can run automation and sub-playbooks with tasks, communicate with end users, set conditions, and store relevant data.

**Standard**
Standard tasks range from manual tasks, like creating a case or escalating an existing case, to automated(could be done w scripts) tasks, such as assigning analysts to cases or enriching indicators.

**Conditional**
Use conditional tasks as decision trees in your flow chart

**Data Collection**
You can use a data-collection task to interact with users through a survey

**Section Header**
These tasks make it easy to identify the playbook's start, various sections, and end.

### Inputs and Outputs in Playbook Cortex

Inputs are data pieces originating from issues or external integrations. These inputs undergo manipulation and enrichment, resulting in outputs

#### Task cheatsheets

a valuable tool for playbook development, offering quick access to the system and custom fields. By clicking on the playbook brackets, developers can efficiently populate task inputs and outputs, simplifying the configuration process and enhancing overall playbook development.

#### Indicator Extraction

is a crucial feature in identifying indicators from various text sources such as email content or war room entries. Troubleshooting involves checking the extraction mode and configuring rules for effective indicator extraction.

Indicator Mode Options:
None
Indicators are not extracted automatically. Use this option when you do not want to further evaluate the indicators.

Inline
Indicators are extracted within the context that indicator extraction runs (synchronously). The findings are added to the context data.

Out-Of-Band
Indicators are extracted in parallel (asynchronously) to other actions. The extracted data will be available within the issue, however, it is not available for immediate use in task inputs or outputs since the information is not available in real-time.

#### Indicator Extractor Defaults

Case Creation (Default: Inline)
Case Field Change (Default: Out-of-band)
Tasks (Default: none)
Manual (Default: Out-of-band)

Indicator Extraction Rules and Options
Enhancing indicator extraction within tasks involves creating rules for efficient playbook execution. Default extraction values are set to inline, and commands like extractIndicators, !ip, and enrichIndicators are used



Use commands like !ad-get-user with raw-response=true for precision in identifying information for extension.



### Filters and transformers

data manipulation is crucial for effective case response, by allowing users to extract, manipulate, and present data in JSON format. Whether applied in playbook tasks or instance mapping, these tools empower users to shape the information according to their specific needs

Filters (can be nested)
Filters serve the purpose of extracting relevant data for further use within the platform.



Transformer
Transformers play a crucial role in data manipulation by focusing on converting or rendering values. eg. A practical illustration involves transforming a date from non-Unix to Unix format or utilizing the count transformer to ascertain the number of elements.
Transformers are further categorized into Date, General, and Custom types:
Date transformers specifically handle the conversion of date formats.
General transformers provide functionalities such as sorting, splicing, and counting.
Users also have the capability to create custom transformers, adding an additional layer of customization to the data transformation process.



Filters in Cortex are categorized based on their functionality. These categories include Boolean, Date, General, String, Number, and Unknown.



### Lists in cortex

A list in Cortex is a container for storing data and is mainly used in playbooks and scripts.
you can access the data in a list via the context button(double-curly brackets) under Lists or by using the path ${lists.<list\_name>}
Different types of data can be stored in a list, including text, string, numbers, Markdown, HTML, CSS, and JSON objects.
eg. of using a list will be that it typically holds IP addresses for whitelisting or blacklisting purposes.



playbook tasks allow you to automate managing lists. You can use playbook tasks to create, view, and manipulate lists.



### Jobs

enabling automation and orchestration of diverse security operations tasks. By configuring a job, *you can execute a playbook in response to specific events or time triggers.*

2 types of jobs :
Time-Triggered Jobs
Feed Triggered Jobs

Where are jobs used in cortex

Creating Indicators List
Vulnerability Management
Indicators Enrichment
System Health Check  
Generating Summary Reports

Job panel views
Chart Panel
Table View
Detailed View



## Cortex Cloud Automation Features



Automation Features operate at the issue level, Analyst will assign targeted actions to run automatically when those conditions are met. These actions are implemented through lightweight playbooks, which are simpler and more focused than those typically used at the case level.

Automation Features is built around four key building blocks.
Triggers - conditions under which a playbook should run
Jobs -
Integrations - such as AWS or Slack can be configured directly from the playbook task where they’re used
Quick Actions -  are single-task automations, like sending a Slack message, closing a ticket, or changing issue severity.

Understanding the Playbook Catalog -> Use-Case-Driven Structure


### Automation Rules

Automation rules define how and when a playbook should run based on incoming issues or specific criteria. These rules replace the older concept of playbook triggers and are designed for greater flexibility and control.

Trigger Types
Issue-Based Triggers - These are automatically activated when new issues are generated that meet specific criteria. Ideal for real time response scenarios
Sub-Playbook Triggers - ensures consistent logic can be applied across multiple playbooks without duplication.
Manual Triggers - useful for one-off investigations, testing automation logic before deploying, or when immediate analyst discretion is needed.



#### Integration Setup in Playbooks

When adopting new out-of-the-box playbooks, there may often be the need to configure integration instances, particularly when those playbooks use high-privilege scripts requiring access to third-party environments like AWS.



### Quick Actions

Quick Actions are lightweight automation components designed for fast, repetitive tasks that don’t require the full logic of a playbook.



#### Ways to Run Quick Actions

Manual Exec ->  manually from the Issues Table by right-clicking on an issue and selecting Run Automation.  - eg .escalating a high-severity issue or alerting another team via Slack.

Automation Rule Trigger -> allowing them to trigger based on defined conditions just like playbooks - eg. Quick Action can be used to auto-resolve known penetration testing (PT) alerts based on user name or description filters.

Tasks w Playbooks -> Quick Actions can be included as individual tasks inside larger playbooks.



### Automating Quick Actions with Rules

Penetration testing activities often trigger a wave of false positives. Analysts are forced to manually triage alerts that are already known and expected, leading to fatigue and distracting from real threats. With Instant Automations, it’s possible to automatically identify and resolve issues generated by known PT activity.



### Cortex working with issues

Issue-related objects that are available for all license types:

Issue
An issue is a notification type of data to report a suspicious activity or an event. They serve as early indicators of security issues or unusual activity in your environment

Case
A case is a container object to group-related issues as well as other attack-related data. Cases represent broader and more complex security challenges, necessitating a coordinated and structured response.

Differences bw issues and cases
issues represent individual security events or conditions, whereas cases aggregate multiple related issues to address more extensive and complex security challenges.
The response intensity varies; issues in Cortex often involve validating the issues themselves, whereas cases demand a comprehensive and structured response that may require the coordination of multiple teams and a range of actions.

Log
A log is any data collection of name-value pairs; values may not be atomic but multiple values.



Issue-related objects that are available with XDR Pro endpoints

Stitched Issues -  is an issue enhanced by Cortex, which is automatically correlated with logs to provide better attack visibility.
Causality Group Owner (CGO) - is the process identified as a root cause of an attack
Causality View - name of mgmt. console page specialized for casualty analysis of stitched issues.

you can access the issues page by entering the URL path: /issues.

External ID Attribute
An issue's External ID is generated by the issues source
In case the source is the Cortex XDR agent, this ID is known as the prevention ID.



Issue attributes
To display the issue's attribute values, press Alt and right-click the issue in the Issues table to open the shortcut menu. Click Debug issue to open a dialog that displays issue attributes in JSON format.



Actions on Issues
An issue's shortcut menu displays the applicable actions whose availability vary depending on the source and category of the issue and whether the issue was stitched with the endpoint data.

some of the actions available for a behavioral threat issue.

Investigate Causality Chain
Here you have the option to open the issue in Causality View or Timeline View in a new browser tab or in the same tab.

Manage Issue
you can select Create exclusion rule to put the rule that caused the Cortex XDR agent to trigger the issue into silent mode so it does not fire issues.

Retrieve Additional Data

Retrieve issue data - process memory dump from the endpoint.
Retrieve related files: of the attack-related files, mainly the executables.

Pivots to Views
Go to Investigate ->  Open Host Risk View to view additional host risk information.



#### Starring issues

You can add a starring policy in Cases \& Issues > Case Configuration >  Starred Issues.

An issue exclusions rule takes precedence over an issue starring rule if the same issue matches to both.



### Featured Issue fields

The featured issue fields are Host, User, and IP Address.
Consider how challenging creating filter criteria for a list of 100 hostnames would be
eg.  you can add a list of hostnames as the featured issue field values of the Host type. Then, to prioritize or better highlight issues that contain any one of the values you added, you use the provided issue fields to sort or filter.

Featured Field Values
To add new featured field values, go to Cases \& Issues > Case Configuration > Featured Fields and click one of the available featured value options: Hosts, Users, and IP Addresses.



### Create Cases

A case is a container object that groups related issues, assets, and artifacts—all of which originate from the same root cause. Each case requires a separate and independent investigation.

Case ID in the Issues Table

Each issue is associated with an issue ID; so, related issues are grouped in the same case.

relates issues based on their attributes that are simply security-related attribute types issues within the same causality instance are related, and they are placed in the same case because causality instance ID (CID column in the Issues table) is a type of cyber attribute.



Binding Issues to Matching Cases

An issue's cyber attributes are security-related or attack-related attributes and include causality ID, IP address, domain, and name of the file and process. Each issue category has its own set of cyber attributes with varying weights.

Cortex binds issues to cases when an issue has just arrived

### Cases

To maintain the relevance and timeliness of cases, Cortex has thresholds for when a case stops accepting new issues and when subsequent related issues are grouped into a new case.

External integrations within Cortex significantly enhance the threat investigation process. One key element is the display of WildFire-issued verdicts for Key Artifacts in each case, WildFire's detailed analysis aids in understanding the nature of the threats, and this information is presented alongside relevant Key Artifacts in the case details to help facilitate more informed investigations.

Cortex cases from the Cases page in the management console.
Table view (Default)
Split-Pane View

1. List pane -  contains consolidated key incident information.
2. Detailed view - organizes case information in specialized tabs



#### Prioritizing and Highlighting Cases

All the rules work at the issue level, and not directly at the case level. However, modifying a case's issues also modifies the case itself. For example, when an issue is starred, the case containing the issue is also starred.



#### Case scoring

Manual (takes precedence)
Rule-Based Score
Smartscore
Create scoring rule for issues and not cases



##### SmartScore Technology

SmartScore uses AI and machine learning to generate reliable risk scores for security cases. It considers:
Context analysis
Case-level feature evaluation
Statistical prevalence comparison
Model-based sub-scores

The system continuously improves by learning from environmental changes and analyst feedback



### Cortex actions

You can initiate three types of actions from the Cortex management console:
response  
To respond to malicious network activities, you can use the external dynamic list (EDL) response action, which adds the malicious domains or IP addresses to the block lists to be consumed by next-generation firewalls (NGFWs).
investigation
you can initiate a Live Terminal session to remotely connect to an endpoint
Maintenance
uninstalling and upgrading the Cortex XDR agents.

Targets of Response Actions
Processes and Files
Actions such as quarantining a file or terminating a process directly target individual processes or files.
Endpoints
Some actions can act on the entire endpoint.
Networks
The EDL action can impact network activities by maintaining block lists of IP

Cortex uses WebSocket to send actions to the Cortex XDR agents



#### Benefits of Tracking Actions

when disconnected endpoints connect later to cortex
the Support File Retrieval action may take up to 10 minutes depending on the size of your log files, memory dump etc

Depending on the action, expiration timeout can be four or 30 days

Agent Isolation
Settings are found within Endpoints > Policy Management > Prevention > Profiles > Agent Settings

Block List Hash Override
Cortex allows you to define an optional setting to terminate hashes on an endpoint regardless of the malware profile settings.

To enable override report mode - From the Investigation \& Response > response > Action Center > Block List page



#### Endpoint response actions

Canceling Endpoint Isolation
in 2 ways-

1. using the Cancel Endpoint Isolation action from the Action Center
2. using the Cytool app with the isolate stop command option



To respond to and further investigate an alert associated with an endpoint, initiate a Live Terminal session that establishes a stateful connection to the endpoint.
Live terminal has 4 interfaces
Task Manager and File Explorer provide remote process management and file management on a connected endpoint respectively.
CLI \& Python are pretty self explanatory

\*Disabling capabilities on an endpoint is an irreversible action. To enable a capability, you must uninstall and reinstall the agent. \*



### Advanced response action

requirements include
Authorization
Script execution requires roles with specific permissions, such as run high-risk scripts and script configuration.
Additional Configuration
Remediation suggestions require enhanced endpoint data collection.
Operation System Support
Remediation suggestions action is available only for Windows.



#### Remediation Suggestion Actions

Cortex enables the reversion of changes made by malicious processes. For example, you can revert maliciously changed files and registry keys



### Scripts

upload Scripts (py 3.7) from Investigation \& Response > Response > Action Center > Agent Scripts Library.



### External Dynamic Lists

An EDL is a type of block list where each entry on a new line represents an IP address or hostname to be blocked.

Cortex instances can serve two types of block lists over Hypertext Transfer Protocol Secure (HTTPS): IP addresses and domain names. Domain names may also include hostnames in fully qualified domain name (FQDN) form.'

The Cortex EDL service has a global configuration located in Settings > Configurations > Integrations > External Dynamic List Integration.

Malicious IP addresses and domains can be added to the EDL lists from multiple pages in the management console, Investigation \& Response > Response > Action Center > Currently Applied Actions > External Dynamic List
Investigation \& Response > Response > EDL > Add to EDL,
and from Cortex CoPilot,
Quick Launcher, and then selecting IP View.

Management of the EDL lists can be done through Investigation \& Response > Response>Action Center > Currently Applied Actions > External Dynamic List.



### Exception

To view new and existing exception rules, navigate to Settings > Exceptions Configuration.
An issue exclusion rule is a rule to suppress issues in Cortex.

Any update made to an issue impacts the associated case



##### Difference between Exclude Issue Action vs Creating an Exclusion Rule

Creation Process
Individual issue exclusions are used to exclude specific issues, typically accomplished via a right-click action. On the other hand, exclusion rules are broader and designed to exclude a set of related issues.

Visibility
Exclusion rules are recorded and can be found in the Issue Exclusions table located at Settings > Exceptions Configurations > Issue Exclusions. In contrast, individual issue exclusions do not appear in this table.

Use Case Example
When un-quarantining a file, an exclusion rule is created to exclude related issues, ensuring a comprehensive approach. This is different from using an individual issue exclusion action, which only targets a specific issue and does not automatically cover related issues.

best practices

Review Exclusions
Audit Exclusions -
Scope Exclusions - avoid overscoping to optimize threat management.
Consolidate Exclusions - where and how you can consolidate overlapping exclusions



#### Exception vs exclusion

An exception disables the underlying protection entirely. Thus, issues aren’t being generated because the protection is removed.

An exclusion silences issues from appearing in the Cortex console but the modules continue to provide protection.While the protection exists, issues are no longer generated from it. Exclusions are used to tune out noisy false positives.

Exceptions profile
Exceptions in the Cortex management console are settings that disable certain protections. By using these exceptions, you can fine-tune security measures to resolve issues such as incompatibilities, crashes, and false positives. For instance, you might need to disable Child Process Protection for a particular process if it crashes due to compatibility issues. After creating exceptions, you can associate them with a policy rule.

Exception Types

Process Exception
The process exception disables selected exploit and malware protection modules on a process to allow a file to run. u can create a process exception to disable selected Exploit Protection Modules (EPMs) and Malware Protection Modules (MPMs) on processes.

Support Exception
Support exceptions are created by the Palo Alto Networks support team to temporarily address policy issues for a specific customer. received in json files

Behavioral Threat Protection (BTP) Rule Exception
A BTP rule exception disables a specific BTP rule to allow a process to keep running.

Local Analysis Rules Exception
A local analysis rules exception disables all the local analysis rules that are involved in creation of a specific issue.

Advanced Analysis Exception
An advanced analysis exception can be created by the cloud-based dump analysis service after analysis of a memory dump file.

Digital Signer Exception
A digital signer exception removes an untrusted signer from the block list to allow files signed by this signer to be able to run.

Network Packet Inspection Engine Exception
A network packet inspection engine exception disables a network packet Inspection rule that is involved in creation of a specific issue.

Credential Gathering Protection Rule Exception
A credential gathering protection rule exception disables a specific credential gathering protection rule across all processes.

Anti-Webshell Protection Rule Exception
An anti-webshell protection rule exception disables a specific anti-webshell protection rule across all processes.

Financial Malware Threat Protection Rule Exception
A financial malware threat protection rule exception disables a specific financial malware threat protection rule across all processes.

In-Process Shellcode Protection Rule Exception
An in-process shellcode protection rule exception disables a specific in-process shellcode protection rule across all processes.

Malicious Device Prevention
A malicious device prevention rule exception protects against connecting potentially malicious devices to the endpoint.

Exception types are created when you perform the Create issue exception action.

Advanced analysis
Dump Analysis Verdict
The verdict from a dump analysis can be one of three types: malware, benign (likely a false +), or inconclusive
If the verdict is benign, an Advanced Analysis exception is created. This exception disables the Endpoint Protection Manager (EPM) that terminated the process.

Navigate to Settings > Configurations > General > Agent Configuration.
These settings allow Cortex XDR instances to connect to the cloud-based dump analysis service.



###### Issue Exception versus Exclude Issue

While both actions result in the absence of a specific issue in the Issues table, their implementations differ.

Issue Exceptions
Issue exceptions are implemented by the Cortex XDR agent. When you create an issue exception, Cortex sends a command to the agent to stop generating any issue of this type. This means the agent itself no longer produces these specific issues.

Exclude Issue
In contrast, issue exclusions do not involve the Cortex XDR agent. The agent continues to create and send issues of this type, but Cortex disregards them in the management console. Essentially, issue exclusions filter out these issues at the management console level without stopping their generation at the agent level.

*You can use the Query Builder to search for data related to issues that have been excluded. This tool allows you to locate and review data even if the issues are not shown in the Issues table.*



#### Global exception

Global exceptions are a powerful tool for managing issues and policies across all endpoints. Global exceptions unconditionally apply to all endpoints.

navigate to Inventory > Endpoints > Policy Management > Prevention > Profiles and select an Exceptions profile to apply exceptions to specific endpoints based on policy rules.



### Logs

Issues
Issues are instantly created to capture details of suspicious activities and are immediately uploaded to a Cortex instance. However, due to their quick creation and processing, issues may not capture all activity details, such as process-resource interactions, making them insufficient for behavioral threat analysis on their own.

management console offers tables for viewing audit logs, including Management Audit, Agent Audit, and XDR Collector Audit within the Settings menu. users can also use XQL

Cortex can correlate issues and logs, which is also known as log-stitching
Causality Chains   |   Timelines

#### Enhanced Endpoint Data Collection

The Cortex XDR Pro Endpoints agents, also known as Pro Agents, can collect enhanced endpoint data (EED) logs on endpoints. is stored in The Cortex Instance
collected logs are uploaded every 5mins

process ops and network ops are the types of data collected in the logs

Stitched issues
You can identify stitched issues in the Issues table. Most causality-related attribute names begin with the CGO prefix. Any non-blank causality-related attribute indicates that the issue has been stitched.

A stitched issue provides advanced investigation and visualization actions: Open Card (entire process execution chain) and Open Timeline (Stages of the attack lifecycle)

You can check the log collection and log stitching status by reviewing the issue attributes displayed in JSON.
keep an eye on these (do these to access the following attributes (Alt + right-click (Windows) -> debug issue))
matching\_status - correlation status of this issue with enhanced endpoint data.
agent\_data\_collection\_status - indicates whether the enhanced endpoint data upload was enabled when this issue was created on the endpoint.

#### Behavioral analysis

The CAE detects and responds to hidden, post-intrusion threats that have evaded network defenses before the attacks ruin the systems.

how the Analytics Engine detects anomalies using behavior analytics
Learning Normal Behavior (using ML)
Comparing Activities to Normal Behavior (compares ongoing activities with the normal behavior)

Some MITRE ATT\&CK tactics are:
Execution
Persistence
Discovery
Lateral Movement
C2C
Exfiltration

Here are some ways in which the Analytics Engine detects attack tactics:

Discovery
Search for symptoms such as changes in connectivity patterns that include increased rates of connections, failed connections, and port scans.

Lateral movement
The engine looks for activities, such as increased administrative activity.

Command-and-Control
Search for unexplained changes in the periodicity of connections, failed and random domain name system (DNS) lookups etc

Exfiltration
Through examination of outbound connections with a focus on the volume of data being transferred.

#### Analytical detectors

The Analytics Engine organizes its behavioral analytics activities by units called detectors
To raise issues, each detector creates its own baseline by processing more than 1,000 data dimensions. The data sources include Cortex XDR agents, Palo Alto Networks Next-Generation Firewalls, GlobalProtect and Prisma Access, and external syslog sources such as Check Point firewalls.

enable the Cortex Analytics Engine in Settings > Configurations > Cortex – Analytics.

### Causality

The Causality View presents the issue and includes the entire process execution. The availability of the "Open Card in new tab" and "Open Card in same tab" depends on whether the issue is stitched chain that led up to the issue.

The Open Card actions are not available for non-stitched issues that are not generated by the XDR agent or Cortex server components.

Causal Analysis
Causal analysis is a type of statistical analysis of event data (issues and logs) to decipher the cause-effect relationships between attackers (malicious processes) and victims (computing resources and assets).

Timeline Analysis
The Timeline provides a forensic timeline of the sequence of events, issues, and informational behavioral indicators of compromise (BIOCs) and correlation rules involved in an attack.

#### Causality chains

An attack region consists of a group of processes within the hierarchical process tree that are involved in an attack. One of the primary challenges in this context is distinguishing which specific processes were part of the attack.

Highlighted view in causality view represents the attack zone, which includes all processes directly or indirectly involved in the attack. its like nodes (symbolized as circle) connected with links

Tree is the data structure where the nodes in the causality instance chain interconnected

#### Causality Instances

Stitched issues can reveal all the processes involved in suspicious activity

Each causally-related process group is called a causality instance (CI) (identified with a CID), also known as a causality group instance, a causality chain, or a causality execution chain.

Causality Group Owner (CGO)
In the case of an attack, Cortex XDR agent terminates all processes in a causality instance, which are considered responsible for the attack. . In the case of a causality instance, the root cause is the group’s root process, which is called a CGO.

Formation of causality instances
Cortex leverages predefined, learned processes called spawners and final spawners to enhance security monitoring through causality instances.

Spawners
To effectively divide the process tree into distinct causality instances, Cortex XDR employs (using ML) special marker processes known as spawner. When new spawners are discovered, they are added to the endpoint list through content updates.

During runtime, as the Cortex XDR agent monitors enhanced endpoint data, it identifies every immediate child process of a spawner as a CGO

Final Spawners
In the context of a single attack, multiple causality instances can be linked. These linked spawners, which may result in dependent causality instances, are identified as final spawners during threat research.

To break any dependencies, the Cortex XDR agent downgrades all potential spawners within the descendant hierarchy of a final spawner to normal processes. This ensures that the causality instances remain independent.

###### Issue Nodes

While a process node represents a single operating system process, an issues node is attached to a specific process node that caused the issues to be generated. Each of the issue nodes in the example are connected to a process node

###### User Nodes

This information is collected by Cortex Identity Analytics, which requires a Cloud Identity Engine instance paired with the Cortex instance and an Active Cortex Analytics Engine.

###### Injection Nodes

An injection node can be a subtype of the process node. In such remote executions, the process node shows an embedded icon to indicate that a RPC or code injection has occurred on that node.

#### Timeline view

Event section
The three high-level categories are Cortex issues in red; BIOCs and correlation rules issues in orange; and other informational activities and events in blue

Unlike Causality View, the Timeline View displays all the issue types, including Informational and Low issues. Informational issues are neither triggered as an attack indicator, nor dispatched to cases. Informational issues are not listed in the issues table on the Issues page, and are not shown in the Causality View.

### Building and Managing Queries

Distinguishing Between Issues and Leads
An issue is an immediate notification that is readily available to you

Leads
A lead, however, is a potential indicator of a threat that requires further definition

Threat Hunting (lead investigation)
Use the tools in the following order -
Query Builder - Search for leads
Causality and timeline -
BIOCs - create XDR rules as BIOCs to create alerts

Since the management console does not provide a dedicated table for logs, search queries are the only way to view logs in datasets.

Template-based questions
Use Query Builder templates to create effective queries without using XQL.

The Query Builder page is accessed via Investigation \& Response > Search > Query Builder.

You can investigate connections among processes, child processes, and endpoints by building a query

Process Entity Search Criteria
Acting (parent) process attributes limit the scope of the search to a specific parent process using attributes such as hashes, user name, signer, or PID.

When you run a search query, regardless of the type, Cortex displays the execution results in the Results table.

Query Center

The Query Center page of the management console displays a table of executed queries. Here is where you can manage your queries. The Query Center is accessed via Investigation \& Response > Search > Query Center.



Show Results and Rerun Query
When you rerun a query, Cortex creates a new query with the same Query Name as the source query.

Query Scheduling
Every time a scheduled query runs in the background, Cortex creates a new query with the same Query Name and Query ID, but with a different Execution ID.

Scheduled queries page
The Query ID is the unique identifier for the Scheduled Queries table, and the Execution ID is the unique identifier for the Query Center table.

### Basic rules

one way to identify threat indicators is to search for leads in Cortex datasets.
Cortex checks incoming log streams against these rules. When a match is found, Cortex generates a detection issue



IOC rules in Cortex define the static properties of threats, such as filenames, paths, file hashes, IP addresses, and domain names. The purpose of IOC rules is to detect known, simple malware.

BIOC rules define the behavioral characteristics of threats, such as the tactics, techniques, and procedures used during the attack cycle. BIOC rules are intended to detect advanced persistent threats such as fileless attacks.

Correlation Rules can be used to identify complex behavioral threat patterns across your network, which may include multiple BIOC characteristics. It is defined by creating Cortex Query Language (XQL) queries against multiple events and datasets that are scheduled to run at certain times.

#### Rule matching

Rule matching is based on two components: rule condition and incoming log streams stored in datasets.

How different datasets are being used by XDR rule engine

The Cortex rule engine constantly checks if the incoming log streams match rule conditions. Incoming logs are eventually stored in Cortex datasets.

Some datasets are created to store only the logs created by the Cortex components, such as the XDR agents. Others store third-party logs.

BIOC rules are checked against the datasets xdr\_data and cloud\_audit\_log, whereas Correlation Rules are checked against all datasets, including third-party log sources.

The dataset xdr\_data contains endpoint logs and alerts that would be stitched with next-generation firewall (NGFW) traffic logs, if integration is provided.

#### IOC rules

IOCs are specific artifacts or patterns associated with threats and serve as fundamental building blocks for threat detection.
Cortex IOC rules are intended to detect known, simple malware. On the management console, you can create and manage IOC rules from the Threat Management > Detection Rules > IOC page.

Key attributes of IOC
Indicator - ip address or hash value
Type - Full path, domain, hash
Expiration date
Reputation - ranging from Completely Reliable to Reliability Cannot Be Judged.

Note that a Cortex IOC rule does not have a name attribute.

#### BIOCs

BIOC rules are intended to detect advanced threats based on behavioral characteristics.

You can create and manage BIOC rules from Threat Management > Detection Rules > BIOC in the management console.



key BIOC rules
Global Rule ID
Name - is unique
Type -  type of the attack event
Behavior - indicates the search condition to extract this issue from the data stream.
severity  - issues created based on the rule definition.

# of issue - number of matches triggered by this rule.

Source - can be the user who created the BIOC
MITRE ATT\&CK Tactic - type of MITRE ATT\&CK tactic that this BIOC rule detected.



Actions on BIOC rules
You can use the context menu to take an action on one or more selected rules. The availability of actions depends on the source.eg. we cant delete if the source is palo alto

Creating a BIOC rule
This is similar to creating a query search, but with the ability to generate issues

A highly recommended best practice is that you test the behavior of a new or edited BIOC rule before saving it

Note that only Medium and High severity issues will generate cases. Low severity issues will appear in the Issues table, while Informational severity issues are only visible in Insights.

#### How Are IOCs and BIOCs Different?

While IOCs are specific artifacts or patterns associated with known threats, BIOCs focus on the behavior or actions of attackers. BIOCs involve monitoring activities such as lateral movement, privilege escalation, anomalous network traffic, or unusual user behavior.
IOCs are valuable for quickly detecting known threats based on their unique characteristics, whereas BIOCs provide a more dynamic and behavior-focused approach to identifying malicious activities that may not have a fixed signature.



#### Custom Prevention Rules Overview

Custom Prevention Rules are BIOC rules that are transferred to endpoints using Restrictions profiles.

To transport the desired BIOC rules to the agents in the form of the Behavioral Threat Protection (BTP) rules, Custom Prevention Rules use Restrictions profiles.

Note that these are asynchronous rules and will kill the process after it is launched. They are not pre-execution rules, meaning the process will start before it's terminated if it matches the rule criteria.

Disabling an Agent or a Server
Disabling the "Agent" will partially disable the rule by removing prevention (protection), while detection capabilities remain on the server. Disabling the "Server" will also partially disable the rule.

#### Suppression Rules Creation

u can create an IOC or BIOC suppression rule to define conditions when not to be matched from Settings > Exceptions Configuration > IOC/BIOC Suppression Rules.
When compared to disabling an IOC or BIOC rule directly from the context menu, the suppression rules are more detailed in their exception definitions.

#### Correlation Rules

Correlation rules in Cortex are designed to analyze correlations of multi-events from multiple sources using the Cortex Query Language based engine for creating scheduled rules.

key capabilities of correlation rules
Flexible Syntax - XQL enables complex correlation logic.
Actionable Alerts -
Alerts can trigger notifications, workflows, and more based on correlation results.
Additional Analysis - Correlation results can be saved as a dataset for further investigation.
Drill Down Query - Rapidly investigate the underlying events after an alert occurs.
Alert Suppression - Prevents duplicate alerts by de-duplicating based on fields and time windows.
MITRE Mapping - Correlations can be mapped to the MITRE matrix.
Export and Import - You can export and import rules and move them between tenants.



Key Components of Correlation Rules

Time Schedule
Time Schedule rules can be set to run on different schedules

Issue Suppression Settings
To prevent issue fatigue, administrators can configure issue suppression for a specified duration and by specific fields after an issue is generated.

Resulting Actions
Correlation rules can be configured using one of the following resulting actions:
Generate issues
Save data to a dataset (useful for testing and fine-tuning new rules)
Add or remove data from lookup datasets

Issues Fields Mapping
When generating issues, fields can be mapped to enrich the resulting case with relevant information.

#### Performance and Monitoring

To ensure optimal performance and effectiveness, correlation rules have certain limitations and monitoring capabilities.

Automatic Disabling of High-Volume Rules
To prevent issue overload, Cortex automatically disables correlation rules that reach 5,000 or more hits over a 24-hour period.

Correlations Auditing Dataset
You can monitor rule executions using the correlations\_auditing dataset,

Query Limitations and Error Handling
The Correlation Rules page helps identify and resolve errors, displaying the number of errors found and allowing filtering to show only rules with errors.

#### Update Lookup Datasets Using

Correlation Rules

This feature allows for the dynamic addition and removal of entries in lookup datasets, enabling users to fine-tune how data from external sources correlates with the events detected in their environment.

#### Update Lookup Datasets Using

the API

making it easier to synchronize and correlate data across different platforms. The supported APIs include:
add\_data: Adds or updates entries in a lookup dataset
remove\_data: Removes entries from a lookup dataset
get\_data: Retrieves entries from a lookup dataset
add\_dataset: Creates a new lookup dataset
delete\_dataset: Deletes an existing lookup dataset
get\_datasets: Lists all available lookup datasets



Cortex provides the following protections -

Threat Visibility
Behavioral analysis
endpoint protection
Network Security Integration
Incident Investigation
Threat intelligence
Scalability
Attack surface mgmt.

Detection rule use case:
Threat detection
Insider threat detection
Compliance Monitoring
Case response

#### Case management

Case management detailed pane view
Overview
The Overview tab offers a case header with details, MITRE tactics and techniques, a summarized timeline, and widgets to visualize various aspects of the case, such as the number of issues, issue sources, hosts, and users associated with the case.

Key Assets \& Artifacts
information about the assets and artifacts associated with the case such as hosts, users, and ke caey artifacts.

Issues \& Insights
displays a table of issues and insights related to the case

Timeline
offers a chronological representation of actions related to the case, including when the case was created, assigned, updated, and resolved

Case War Room
Real-time investigation is facilitated through the Case War Room, powered by ChatOps. Analysts can use Command-Line-Interface (CLI) commands to perform tasks related to case investigation,

Executions
The Executions tab displays the Causality View of different issues inside the case, which provides a powerful way to analyze and respond to issues.



Resolution Reasons

True Positive
False Positive
Security Testing
Known Issue
Duplicate Case

Creating Custom Case Fields
navigate to Settings > Configurations > Object Setup > Cases > Fields. Click the + New Field button to add a new customized field.

Custom Cases Layout Main View
Navigate to Settings > Configurations > Object Setup > Cases > Layouts. Select + New Layout to add a new customized field.

#### SLA

Cortex allows organizations to efficiently track Key Performance Indicators (KPIs) associated with cases by using the Case SLAs feature.

Determine SLA Goals by Cases
Identify what types of cases need SLAs and how you are going to track SLA measurables for the cases. Then, determine the time limits to complete the SLA actions.

Create a Timer Field
The Timer field requires both a case filter and conditions to determine when the timer should start. Once the timer condition is met, the SLA goals come into effect. The Timer field counts forward, while the SLA field counts backward.

use Case SLAs, let's look at a potential use case where an organization wants to ensure that cases are moved from status New to Under Investigation within specific time durations based on their severity level (e.g., critical < 60 minutes, high < 120 minutes, medium < 12 hours).

Additional info on SLA

Case Resolution
When a case is resolved, the timer calculation stops.

Updating the Timer Logic
Updating timer logic affects open and new cases. Therefore, the timer and associated SLA will be removed from any case that no longer qualifies, even if the timer is already running.

SLA in Breach
When a case is in violation of its SLA, its SLA field shows a red time clock icon. Cases still within their SLAs show a black time clock icon in this field.

Deleting a Timer Field
If you delete a timer field, the SLA associated to the timer is also deleted.

XQL Queries
Timer and SLA data is available in XQL queries.

War Room CLI Command or Playbook Task
You can update Timer and SLA fields by running the following CLI command in the War Room:
!RefreshIncidentDynamicCustomFields

Artifacts and Assets
Investigate IP
It offers a consolidated summary of all available information about a specific IP address over a defined time frame

Investigate Assets
It automatically aggregates host information and displays insights, making it easier to assess the status of assets.

Investigate File and Process Hashes
The Hash View in Cortex is designed to expedite the investigation of SHA256 hash processes and files. It compiles and presents a summary of all available information regarding a specific hash over a chosen time frame (24 hours or 7 days).

### War rooms

War Rooms in Cortex centralize all investigation actions, entries, and collaboration related to playbooks. Think of War Rooms as interactive timelines of your automated case response, capturing every action and decision taken. Each case and issue has its own unique War Room.

#### Case War Room

The Case War Room is used to manage and coordinate the response to confirmed security cases.  The Case War Room provides a centralized space for collaboration among case responders, allowing them to share information, assign tasks, track progress, and communicate effectively during the case response.

#### Issue War Room

The Issue War Room is designed for initial triage and investigation of security issues. When an issue is triggered, it is routed to its War Room where analysts can assess the severity and relevance of the issue

To run automation scripts, commands that are supported by the enabled integration, or system commands, use ! commands.
To contact other team members to collaborate on a case, use @ commands.

#### Context data

The context for an issue/case is a JSON-formatted repository for output data from automation, integration, and playbook commands. The context also stores all data that is associated with the creation of the issue/case.

Context data serves as a foundation for two essential functions (Capture Data \& Share Data) used by analysts, Cortex engineers, playbook tasks, and automation scripts.

#### Forensic

Cortex forensic module is a XSIAM and XDR addon

create investigation
create collection
Hunt - collection of builtin or configurable searches including artifact searches, when u wanna search for a specific activity across many endpoints  
Triage - More focused collection, captures past forensic artifacts and raw files from selected endpoints where u need additional info about a specific activity , they collect detailed system info  
Close Investigation

### CORTEX PORTFOLIO

The Problem: Too Much Info, Too Many Silos, Not Enough Insight
For example, the network's data goes with the Network Traffic Analysis (NTA) system and the endpoint data goes into XDR. Some logs feed into the Security Event and Incident Management (SIEM) system, while the identity data is somewhere else

Siloed Tools and Data
Switching between multiple products and consoles can result in increased cognitive load, and potential oversight of threats.

Inability to Stop Threats at Scale
Relying solely on static correlation rules and extensive detection engineering, that is exacerbated by the volume of data, poses challenges in identifying meaningful relationships between security events across the environment.

Heavy Reliance on Manual Work
Oftentimes, they may be investigating several different alerts, without knowledge that the alerts are connected to a single incident



#### Cortex XDR

Cortex XDR unifies prevention, detection, and response across all data sources, offering powerful network visibility and custom network-based detection rules for Next-Generation Firewall (NGFW) customers.
Cortex XDR leverages global analytics and network traffic analysis to detect stealthy attacks

core functions -
Prevent Known and Unknown Threats
Detect advanced threats
Reduce Investigation Time

At the heart of the prevention capability is the Cortex XDR agent. This agent is designed to block various types of attacks directly on the endpoint

At the heart of the prevention capability is the The platform includes a sophisticated Behavioral Threat Protection engine.

Cortex XDR is fully integrated with WildFire, Palo Alto Networks' cloud-based malware prevention service.

Reducing investigation time

Automated Data Correlation
Integrated Threat Intelligence - from WildFire and VirusTotal is automatically integrated into the incident view.
Incident Grouping - Each incident provides a comprehensive overview, including:
Key artifacts involved in the attack
Threat intelligence details
User and host information

#### XSOAR

unifies case management, automation, real-time collaboration, and threat intel management to serve security teams across the case lifecycle.

Cortex XSOAR ingests aggregated issues and indicators of compromise (IoCs) from detection sources, such as SIEM solutions, network security tools, threat intelligence feeds, and mailboxes. After data ingestion, Cortex XSOAR can execute automatable, process-driven playbooks to enrich and respond to these cases. These playbooks coordinate across technologies, security teams, and external users for centralized data visibility and action.

Benefits of XSOAR
Improve SOC Efficiency by Automating Case Response
Automate case response workflows for common use cases with 1000+ pre-built integrations and playbooks
Enterprise-Ready Performance and Scalability
Cloud-native SOAR auto-scales to support future growth
Rapid deployment accelerates return on investment (ROI)
Ingest, Search, and Query all Security Issues
Unify issues, cases, and indicators from any source into a single platform for lightning-quick analyst investigation and response.
Speed Remediation with Integrated Case Management
Built-in collaborative functions such as War Room for every case, ChatOps, and integrations with customer success management (CSM) tools enables teams to manage the full case lifecycle in a single location.
Operationalize Threat Intelligence
Unify aggregation, scoring, and sharing threat intelligence with playbook-driven automation with native threat intelligence management

#### XSIAM

Cortex XSIAM is a cloud-delivered, unifies key functions, including EDR, XDR, SOAR, ASM, user and entity behavior analytics (UEBA), TIM, and SIEM.

It eliminates the hassle of console switching, providing a streamlined experience. The platform offers broad integration support, making it easier to onboard various data sources without the need for extensive engineering and infrastructure work.

#### Cortex cloud

Cortex Cloud delivers both cloud and endpoint security with its unified agent. Its AI-ready data and MITRE ATT\&CK technique-level detection coverage provides maximum threat visibility

To address the growing security challenges organizations face in the cloud, the Cortex platform offers Cortex Cloud, a unified cloud security platform that integrates the Cortex and Prisma Cloud capabilities to provide comprehensive, real-time security from code to cloud to SOC.

the merging of the Cloud Native Application Protection Platform (CNAPP) and the leading SecOps platform into a single, powerful solution

###### Application Security

Cortex Cloud integrates with engineering ecosystems to secure applications and prevent risks by combining leading AppSec tools with third-party scanners for full risk context.

Centralized Visibility
Application Security Posture Management (ASPM) provides visibility across AppSec tools and third-party scanners for consistent security across the lifecycle.

Ecosystem Security
Secure the engineering ecosystem—code, supply chain, and tools from a single platform.

Agile Guardrails
Prevent risk from reaching production with agile guardrails that enable developers to automatically apply best practices from within native developer tools.

Risk Management
Manage risks with combined context from code, pipelines, runtime, and applications, prioritizing based on exploitation probability and business impact.

###### Cloud posture security

AI-Driven Security Posture
Cortex Cloud centralizes, automates, and scales cloud security to measurably improve the detection, prioritization, and remediation of cloud risks. Key features include:

Comprehensive View
Attack Path Detection
Intelligent Risk Correlation
AI-Driven Resolution
Compliance and Reporting
Capability Consolidation
Consolidate capabilities typically found in Cloud Security Posture Management (CSPM),
Cloud Infrastructure Entitlement Management (CIEM), Data Security Posture Management (DSPM), Artificial Intelligence Security Posture Management (AI-SPM), and vulnerability management tools.

###### Cloud Runtime Security

Features are
Cloud workload protection
CWP enhances cloud visibility, analyzes vulnerabilities, and ensures compliance.
Web app \& API security (WAAS)
Cortex's Web Application and API Security features ensure only authorized access, enhance security beyond firewalls, and protect data.
Cloud detection \& response (CDR)
CDR, enabled by Cortex XDR Agent for Cloud, provides advanced threat detection, automated response, and AI analytics for cloud environments, including threat hunting.

#### Cortex instance

A Cortex instance is created per tenant, which can be an organization, a company, or a team. The instance is created in Google Cloud Platform through instance activation. Instance activation and tenant activation refer to the same task. is activated on cortex gateway

Authentication and authorization (A2) are required to interact with Cortex instances. For example, to access the Cortex management instance, you must have a CSP user account as well as the required CSP and Cortex roles. Customer Support Portal and Cortex Gateway manage A2.

Gateway is for authorization, controlling RBAC

URL to access cortex instance - https://<"subdomain">.xdr.<"region">.paloaltonetworks.com

##### Support PAN

For example, you can view your Cortex and Strata Logging Service licenses (description, serial number, expiration date, install region, etc.) at Products > Cloud Services.

#### Console management Console

Cortex management console to interact with the instance
You can directly access the Cortex management console from your Cortex Gateway or by using the Fully Qualified Domain Name (FQDN) specified during the instance activation.

Palo Alto Networks sites that require authentication use single sign-on (SSO) based on Security Assertion Markup Language (SAML), which allows users to sign into various web applications based on their sessions created in another context. A SAML session is valid for eight hours



In the Settings > Configurations > General > Security Setting u can also setup session and user expiration

Main Components of Dashboards \& Reports
Dashboard
contains widgets that summarize information about endpoints in graphical or table format.
Reports
Reports can be set to output to email or Slack
Dashboards Manager
Use the Dashboards Manager to set the default dashboard or custom-create new ones.
Reports Template
Widget Library

You can view custom and default dashboards in Dashboards \& Reports > Dashboard Manager. You can edit, disable, duplicate, or delete dashboards from the Dashboard Manager section.

### Common Compliance Regulations

GDPR
PCIDSS
HIPAA
Sarbanes–Oxley

Sarbanes–Oxley (SOX) refers to the U.S. Securities and Exchange Commission’s data requirements around US company financial accounting.

Download different compliance reports and dashboards in the Marketplace.

Here are four of the many compliance standards and programs that Palo Alto Networks adheres by.

Service Organization Control Type 2 (SOC 2)
SOC 2 is a compliance framework developed by the American Institute of Certified Public Accountants (AICPA). They ensure that third-party service providers store and process client data securely.

C5
C5 (Cloud Computing Compliance Controls Catalogue) is the “cloud computing IT-Security” standard in Germany. Designed and released by the BSI in February 2016, the C5 control set offers additional assurance to customers in Germany as they move their complex and regulated workloads to Cloud Computing Service providers such as AWS.

Federal Risk and Authorization Management Program (FedRAMP)
FedRAMP is a government-wide program that provides a standardized approach to security assessment, authorization, and continuous monitoring for cloud products and services.

International Organization for Standardization (ISO)
The organization develops and publishes international standards in all technical and nontechnical fields other than electrical and electronic engineering. Palo Alto Networks supports ISO 27001, 27701, 27017, 27018.

### Infrastructure

and Access

A Cortex instance requires cloud-based included and optional Cortex infrastructure services from Palo Alto Networks.

WildFire
WildFire is an included service that provides threat intelligence in the form of real-time threat analysis for unknown files and ready-to-use verdicts for known files.

Cloud Identity Engine (CIE)
CIE is also an optional service, which provides identity information to Cortex by pulling organizations’ directory information.

Cortex is a regional, cloud-based product; a Cortex instance and instances of its infrastructure services must be bound to the same specific region. The regional cloud architecture ensures that all Cortex-related data collected from users of a specific region remain in that region.

The Palo Alto Networks Hub is the user interface for the is primarily used to manage the CIE cloud service.

#### Cloud Identity Engine

CIE, formerly known as Directory Sync Service, is a cloud service for read-only access to an on-premises directory information of organizations. The directory can be Azure Active Directory (AD). The service is used by a Cortex instance to access identity data.

A CIE deployment consists of two components: a cloud-based CIE instance and on-premises Windows agents connected to domain controllers.

The service instance periodically queries the CIE agents to get a full snapshot of the attributes for the objects, such as computers, groups, users, and organizational units.

A CIE instance and a Cortex instance are paired from the Cortex management console by navigating to Settings > Configurations > Integrations > Cloud Identity Engine.

#### Access control

Roles are managed via Cortex Gateway or the Cortex management console.

To create a new Cortex role, navigate to Settings > Configurations > Access Management > Roles

Scope-Based Access Control (SBAC)

SBAC restricts user access to the functional areas of the management console per scope.

Scope is managed using tags, representing different scoping methods available in the management console.

To configure SBAC, go to Settings > Configurations > Access Management > Users in the management console.

### Cortex Copilot

Cortex Copilot's design addresses these issues by using an LLM chat interface to directly interact with the monitoring, management, and data backend that powers Cortex XSIAM and the Cortex platform.

requires an XSIAM Enterprise or XSIAM Enterprise Plus license \& features are determined by region-based default settings and users' RBAC permissions.

#### The Cortex Copilot Architecture

User Prompts -> Copilot Translations -> Cortex backend -> Backend Responses

Responsible AI

Security
Privacy
No customer data is used to train the Cortex Copilot model.
Explainability
Cortex Copilot responses build in accountability and allow users to provide feedback.

Using an LLM-powered AI assistant to access XSIAM's robust feature set gives Cortex Copilot a distinctive collection of core capacities.

Asset and Artifact Insights
Cortex Copilot can provide data enrichment and contextual detail

Investigation Paths
Asking plain language questions about Cortex data.

Response Options
Cortex Copilot provides users contextualized response options based on what entities or information the user examines.

Knowledge Base
Cortex Copilot can answer user questions by providing summarized knowledge base information or links to the Cortex Help Center.

#### RBAC Controls

The information provided to users on assets and artifacts in an environment is controlled by the user's existing RBAC permissions.



Cortex Copilot hotkey, which is Ctrl+Shift+X on Windows or CMD+Shift+X on macOS.
Users can change the Cortex Copilot hotkey by accessing Keyboard Shortcuts menu at Settings > Configurations > Server Settings.

In copilot options are grouped in the categories Investigate, Respond, and Navigate.

###### AI-Assisted Investigation and Support Ticket Creation

Copilot can assist a user by automatically attaching troubleshooting information in the Cortex Copilot Support Case Wizard
For example, when a user selects they are having an issue with an endpoint, Copilot can automatically attach the relevant endpoint data.

### Bring your own keys (BYOK)

BYOK enables XSIAM users to utilize their own keys for encrypting tenant data at rest. BYOK is set up during the tenant activation process.

Challenge
When an organization needs to import or manage keys, a technical support ticket is submitted, requiring the involvement of a DevOps team at Palo Alto Networks, resulting in a time-consuming dialog.

Solution
Cortex self-managed BYOK simplifies the process for organizations to securely import and manage their encryption key via the Cortex Gateway by providing a new interface to import and rotate keys.

Benefits -
Maintain Greater Control
Eliminate Reliance
Meet Compliance Requirements
Minimize External Risks

built to provide greater control, replace the need for an external KMS, and reduce external risks. In addition, BYOK includes features such as *rotating keys* and audit logs to enhance overall security and aid in compliance with regulations.

#### Requirements of BYOK

Availability and Licensing
Organizations  must have a license for the following products:
XSIAM: All License Types
XDR:  Via a Feature Flag
XSOAR: Via a Feature Flag

Symmetric Encryption Key
The organization needs to generate a 32-byte symmetric encryption key as unencoded binary data

Complete Process Within Three Days
process of importing the key process of importing the key should be done in 3 days

#### Architecture

The BYOK architecture consists of two components:
a dedicated KMS
Cortex BYOK utilizes a dedicated KMS in the organization's GCP tenant.
Two Encryption Keys
Cortex BYOK uses two keys to encrypt data at rest. One key is for BigQuery, and the other is for all the other services within the tenant.

view the workflow steps for importing a key.



Step 1: Generate New Key
In step 1, the administrator generates a new symmetric key and creates an import job that defines the target key ring and key for imported key material.
The public key is used to wrap the key to be imported
The private key is stored within Google Cloud and is used to unwrap the imported key later
The separation prevents Google from unwrapping the key material outside of the scope of the import job.

Step 2: Download Key Ring
The key ring, which encompasses a collection of encryption keys such as wrapping keys or symmetric keys, is downloaded from GCP KMS.

Step 3: Encrypt the Key
The symmetric key is encrypted with the private key from the key ring.
The key is wrapped using rsa-oaep-3072-sha256 encryption for secure transmission.

Step 4: Upload New Key
the administrator uploads the newly generated key, which is transmitted over secure channels using TLS.
The key material is unwrapped using the import job's private key and is inserted as a new version of the target key on the target key ring.
This is an atomic operation, which means it cannot be altered or modified to provide additional control and protection.

Disabling Keys
Disabling encryption keys also occurs in the Cortex Gateway by opening the additional options menu next to the tenant and selecting Disable All Keys \& Deactivate Tenant. Renders the tenant inaccessible .
Only a user with an Account Admin role can disable encryption keys and deactivate a tenant.



### Multitenancy

Multitenancy is a software architecture where a single instance of an application serves independent groups of users, known as tenants.

Cortex XDR and XSIAM have two types of tenants: main tenant and child tenant. 

The main or primary tenant acts as the central administration point for the multitenant environment. 
A child tenant represents an individual customer or a subdivision environment within the multitenant system. Each child tenant has its own isolated instance of Cortex XSIAM

MSSPs and large enterprises are the two primary consumers of XSIAM's multitenancy features.

##### Licensing Models 

customer-owned licensing 
Also known as "Co-Managed", this model is characterized by individual tenants procuring their own XSIAM licenses. 

central licensing.
Also known as "Fully Managed", this model allows the parent service provider (typically a large enterprise) to purchase a main tenant license as well as a pool of resources for child tenants (enterprise subsidiaries). 


Example where the managed security service provider(MSSP) has purchased an XSIAM license for only the parent tenant.
Each of the managed tenants (A, B, and C) are independent companies who obtain their own XSIAM licenses and contracts. When these companies wish to be managed by the MSSP, they undergo a manual pairing process between their individually licensed XSIAM tenant and the MSSP parent tenant. 


The central licensing model is generally intended for large enterprise customers who want to centrally manage security operations for their distributed and segregated subsidiary companies.

This central license is purchased by the enterprise entity. Then, the account administrator of the parent tenant allocates the license pool to each of the subsidiary child tenants as necessary.

Central Licensing Model Dynamic License Allocation

In an organization using the central licensing model, an administrator can manage the following aspects of their child tenants. 

Tenant Allocations
Increase and decrease resource allocations for each child tenant as needed through the Cortex Gateway.

Adding tenants
If resources are available, new child tenants can be added to the main tenant through the Cortex Gateway.

Deleting tenants
When a child tenant is deleted, all its data is permanently removed, and the allocated resources return to the main tenant pool.

Child tenants can be added through the Cortex Gateway by account administrators.
https://learn.paloaltonetworks.com/learn/courses/306/cortex-xsiam-features/lessons/7633:1373/cortex-xsiam-multi-tenant-license-management

#### Multitenancy Capabilities 

Security configuration is the ability of the parent tenant to apply security configurations in the child tenants as needed.

Note the following areas where parent tenants have visibility and can apply security configurations on child tenants.

Parent to Child Visibility Areas
Incidents
Alerts
Query Builder
Query Center and Results
Causality and Timeline View
Forensics
Playbooks

Parent to Child Security Configurations
Behavioral indicator of compromise (BIOC)
Rules and Exceptions
Starred Alert Policies
Alert Exclusions
Endpoint Prevention Profiles
Allow and Block Lists

 XSIAM multitenancy visibility offers the benefit of being able to view information on one or more tenants from the parent.

Security actions are managed by configurations you create in Cortex XSIAM and then assigned to each of the child tenants. Each action requires its own configuration and allocation to a child tenant.

Cortex XSIAM gives MSSPs the ability to manage the content on child tenants by using remote content repositories

MSSPs can always use the Tenant Navigator feature to switch into the child tenant in order to take any actions that they may need to take.

XSIAM Multitenancy also allows you to query data sets in the child tenants.



### References
