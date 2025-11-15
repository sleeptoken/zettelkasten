
2025-11-03 18:24

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
#### Work Plan 

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

#### Playbook tasks

Depending on the task type, playbooks feature inputs and outputs crucial for task execution. Sub-playbooks, categorized as parent or child playbooks, provide modularity and reusability in playbook development.
##### Metadata

Understanding playbook metadata helps in troubleshooting system performance issues, users can analyze playbook task i/p & o/p, storage usage and task types.
- Navigate to the Case War Room on the Cases page and run the following command:
```
!getInvPlaybookMetaData caseId=<case ID> minSize=<size of the data you want to return in KB. Default is 10>
```
##### Task Execution and Management

Playbooks are executed when cases are ingested, automatically running through their predefined workflows.
Users can view and update playbooks associated with cases or issues in the work plan section.
##### Auth for comms tasks

Organizations can configure SSO by setting up dedicated groups of external users in their identity provider (IdP) such as Okta. Parameters such as Single Sign-On URL, Audience URI (Unified Resource Identifier), and IdP SSO URL are specified to establish a secure authentication process.

##### Adding an Ad-Hoc Task

Within the work plan, you can create tasks for a specific iteration of a playbook. The task type can be an automation or another playbook. For example, within a manual task, you might need to enrich some data when running an investigation playbook.

#### Playbook task types

You can run automation and sub-playbooks with tasks, communicate with end users, set conditions, and store relevant data.

**Standard**
Standard tasks range from manual tasks, like creating a case or escalating an existing case, to automated(could be done w scripts) tasks, such as assigning analysts to cases or enriching indicators.

**Conditional**
Use conditional tasks as decision trees in your flow chart

**Data Collection**
You can use a data-collection task to interact with users through a survey

**Section Header**
These tasks make it easy to identify the playbook's start, various sections, and end.

#### Inputs and Outputs in Playbook Cortex

Inputs are data pieces originating from issues or external integrations. These inputs undergo manipulation and enrichment, resulting in outputs

##### Task cheatsheets

a valuable tool for playbook development, offering quick access to the system and custom fields. By clicking on the playbook brackets, developers can efficiently populate task inputs and outputs, simplifying the configuration process and enhancing overall playbook development.

##### Indicator Extraction

is a crucial feature in identifying indicators from various text sources such as email content or war room entries. Troubleshooting involves checking the extraction mode and configuring rules for effective indicator extraction.

Indicator Mode Options:
None
Indicators are not extracted automatically. Use this option when you do not want to further evaluate the indicators.

Inline
Indicators are extracted within the context that indicator extraction runs (synchronously). The findings are added to the context data.

Out-Of-Band
Indicators are extracted in parallel (asynchronously) to other actions. The extracted data will be available within the issue, however, it is not available for immediate use in task inputs or outputs since the information is not available in real-time.

##### Indicator Extractor Defaults

Case Creation (Default: Inline)
Case Field Change (Default: Out-of-band)
Tasks (Default: none)
Manual (Default: Out-of-band)

Indicator Extraction Rules and Options
Enhancing indicator extraction within tasks involves creating rules for efficient playbook execution. Default extraction values are set to inline, and commands like extractIndicators, !ip, and enrichIndicators are used

Use commands like` !ad-get-user` with `raw-response=true` for precision in identifying information for extension.

### Filters and Transformers

Data manipulation is crucial for effective case response, by allowing users to extract, manipulate, and present data in JSON format. Whether applied in playbook tasks or instance mapping, these tools empower users to shape the information according to their specific needs

**Filters (can be nested)**
- Filters serve the purpose of extracting relevant data for further use within the platform.

**Transformer**
- Transformers play a crucial role in data manipulation by focusing on converting or rendering values. 
	- eg. A practical illustration involves transforming a date from non-Unix to Unix format or utilizing the count transformer to ascertain the number of elements.
- Transformers are further categorized into Date, General, and Custom types:
	- Date transformers specifically handle the conversion of date formats.
	- General transformers provide functionalities such as sorting, splicing, and counting.
	- Users also have the capability to create custom transformers, adding an additional layer of customization to the data transformation process.

Filters in Cortex are categorized based on their functionality. These categories include Boolean, Date, General, String, Number, and Unknown.

### Scripts 
Scripts within Cortex serve as potent tools for executing specific actions through commands. A script is attached by default, making it uneditable. To modify a script, users can detach it or create a duplicate. 

## Lists in cortex

A list in Cortex is a container for storing data and is mainly used in playbooks and scripts.
you can access the data in a list via the context button `(double-curly brackets`) under Lists or by using the `path ${lists.<list\_name>}`
Different types of data can be stored in a list, including text, string, numbers, Markdown, HTML, CSS, and JSON objects.
eg. of using a list will be that it typically holds IP addresses for whitelisting or blacklisting purposes.

playbook tasks allow you to automate managing lists. You can use playbook tasks to create, view, and manipulate lists.

You can manage lists in Cortex using various commands through the built-in CLI, automation, and playbook tasks. The CLI feature is located at the bottom of the War Rooms interface. It allows you to execute commands and scripts and has a variety of list manipulation commands.
### Jobs

enabling automation and orchestration of diverse security operations tasks. By configuring a job, *you can execute a playbook in response to specific events or time triggers.*

Two types of jobs :
- Time-Triggered Jobs - at a predetermined time or at regular intervals 
- Feed Triggered Jobs - by changes in the feed

Where are jobs used in cortex

- Creating Indicators List
- Vulnerability Management
- Indicators Enrichment
- System Health Check  
- Generating Summary Reports

Job panel views
- Chart Panel
- Table View
- Detailed View

## Cortex Cloud Automation Features

Automation Features operate at the issue level, Analyst will assign targeted actions to run automatically when those conditions are met. These actions are implemented through lightweight playbooks, which are simpler and more focused than those typically used at the case level.

Automation Features is built around four key building blocks.
- Triggers - conditions under which a playbook should run.
- Jobs - automation process that run on a fixed schedule.
- Integrations - such as AWS or Slack can be configured directly from the playbook task where they’re used
- Quick Actions -  are single-task automations, like sending a Slack message, closing a ticket, or changing issue severity.

Understanding the Playbook Catalog -> Use-Case-Driven Structure

- When a playbook is adopted, the system installs its underlying Marketplace content pack in the background. These playbooks can be reviewed, customized, and edited, all within the same interface, without the need for context switching.
- Out-of-the-box playbooks are locked for direct editing to preserve the integrity of the original template. To modify one, it must first be adopted and then duplicated.

### Automation Rules

Automation rules define how and when a playbook should run based on incoming issues or specific criteria. These rules replace the older concept of playbook triggers and are designed for greater flexibility and control.

Trigger Types
- Issue-Based Triggers - These are automatically activated when new issues are generated that meet specific criteria. Ideal for real time response scenarios
- Sub-Playbook Triggers - ensures consistent logic can be applied across multiple playbooks without duplication.
- Manual Triggers - useful for one-off investigations, testing automation logic before deploying, or when immediate analyst discretion is needed.

#### Integration Setup in Playbooks

When adopting new out-of-the-box playbooks, there may often be the need to configure integration instances, particularly when those playbooks use high-privilege scripts requiring access to third-party environments like AWS.
### Quick Actions

Quick Actions are lightweight automation components designed for fast, repetitive tasks that don’t require the full logic of a playbook.

Ways to Run Quick Actions

- Manual Exec
	- manually from the Issues Table by right-clicking on an issue and selecting Run Automation.  - eg .escalating a high-severity issue or alerting another team via Slack.
- Automation Rule Trigger 
	- allowing them to trigger based on defined conditions just like playbooks - eg. Quick Action can be used to auto-resolve known penetration testing (PT) alerts based on user name or description filters.
- Tasks w Playbooks 
	- Quick Actions can be included as individual tasks inside larger playbooks.

### Automating Quick Actions with Rules

Penetration testing activities often trigger a wave of false positives. Analysts are forced to manually triage alerts that are already known and expected, leading to fatigue and distracting from real threats. With Instant Automations, it’s possible to automatically identify and resolve issues generated by known PT activity.


### References
