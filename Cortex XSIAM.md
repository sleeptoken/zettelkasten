
2025-11-03 18:24

Source: #paloalto 

Tags: [[Defence]]
### Automation Foundations

to streamline and optimize various cybersecurity and security operations tasks within the Cortex platform



#### Automation components

Marketplace - is the central hub for managing content packs, which enhance security operations with content such as playbooks, integrations, scripts, and more.It also helps optimize case and asset management.
Playbook - orchestrating a sequence of tasks, conditions, automations, commands, and loops to streamline and enhance investigation and response processes. *reduces investigation time*.
Filters and Transformers - allowing users to extract, manipulate, and present data in JSON format
Scripts - Scripts within Cortex serve as potent tools for executing specific actions through commands.



### Benefits of Automation

Rapid Case Response - Automation allows for real-time detection and immediate response to security cases
Consistency - ensure that case handling procedures are executed consistently, reducing the risk of human error and ensuring compliance
Improved Efficiency - streamlines routine and repetitive tasks
Scalability  
Reduction of Issue Fatigue - by automating the initial triage and categorization of security issues allowing security analysts to prioritize high-priority cases.
Resolution/Closure -  can help facilitate the rapid resolution of security cases



### Content Pack Features in marketplace

Cortex provides the full potential of automation through a rich set of features that empower organizations to fortify their security posture and streamline case management.

#### Integrations

Integrations connect third-party security and alert management platforms to Cortex. This connection enables these platforms to trigger events, which are then transformed into issues within Cortex, initiating automated security processes.



#### Playbooks \& Scripts

In Cortex, playbooks efficiently automate case handling, minimizing response times and ensuring consistency, while scripts provide customized automation tailored to organizational requirements.
Playbooks are triggered by ingested issues, enabling the definition of a sequence of actions for specific case types. They offer adaptability and resource allocation optimization.



Scripts are composed of a series of commands that can be employed in playbook tasks and when running commands in the issue War Room.



#### Issue Types and Fields

Issue types are essential for categorizing and prioritizing issues within Cortex, serve as the first step in classifying issues, determining the most suitable response and actions for each issue. Each issue type corresponds to a specific set of issue fields, ensuring the association of relevant information with each issue

Issue fields provide detailed information about each issue, capturing specific data relevant to the corresponding issue type.



#### Rules

Correlation Rules analyze multiple events from various sources using the XQL-based engine within Cortex. They are scheduled and utilize the XQL engine, allowing for rule-based threat detection.

Data Model Rules play a pivotal role in data normalization and enrichment. They allow the creation of a standardized schema with predefined data types and the mapping of third-party data to this schema. (making data from various sources easily consumable and consistent in format.)

Parsing Rules
Enable the preprocessing of incoming data, allowing for the removal of unnecessary information, data quality improvement, and storage optimization.



#### Dashboards and Reports

Dashboards consolidate the view of security operations, enabling prompt decision-making and enhancing situational awareness. w  Customizable widgets
Reports facilitate data-driven decision-making and the communication of security insights to relevant parties.



### Content pack mgmt

Content packs undergo regular updates to address bugs, introduce enhancements, and refine their functionality.



### Playbook Overview

Playbooks come into play during the investigation and response phase of the issue lifecycle

Playbooks and case lifecycle

1. Investigation and response
2. Design use cases (The use case is the description of what it is that the SIEM should be alerting on)
3. Create and update playbook lifecycle
4. Issue Handling
5. Define Logical flow
6. Remediations criteria
7. Continuous improvement

Types of playbook
Manual tasks
Conditional tasks - validate conditions based on parameters
Communication tasks - facilitate interaction w others
Automation tasks - automatically remediate issues by engaging third-party integrations, ticketing systems, or file detonation using a sandbox



### work plan options to manage a playbook

Work plans are a part of issues, and contains the playbooks that are slated to be run for that issue.

##### work plan options

Change the playbook
Follow a Playbook - see running in real time
Run Again
zoom
Export Playbook - in png



#### playbook management

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

Understanding playbook metadata helps in troubleshooting system performance issues.
Navigate to the Case War Room on the Cases page and run the following command:
!getInvPlaybookMetaData caseId=<case ID> minSize=<size of the data you want to return in KB. Default is 10>



#### Task Execution and Management



Playbooks are executed when cases are ingested, automatically running through their predefined workflows.



#### Auth for comms tasks

Organizations can configure SSO by setting up dedicated groups of external users in their identity provider (IdP) such as Okta. Parameters such as Single Sign-On URL, Audience URI (Unified Resource Identifier), and IdP SSO URL are specified to establish a secure authentication process.

#### Adding an Ad-Hoc Task

Within the work plan, you can create tasks for a specific iteration of a playbook. The task type can be an automation or another playbook. For example, within a manual task, you might need to enrich some data when running an investigation playbook.

### Playbook task types

You can run automation and sub-playbooks with tasks, communicate with end users, set conditions, and store relevant data.

Standard
Standard tasks range from manual tasks, like creating a case or escalating an existing case, to automated(could be done w scripts) tasks, such as assigning analysts to cases or enriching indicators.

Conditional
Use conditional tasks as decision trees in your flow chart

Data Collection
You can use a data-collection task to interact with users through a survey

Section Header
These tasks make it easy to identify the playbook's start, various sections, and end.

### Inputs and Outputs in Cortex

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




### References
