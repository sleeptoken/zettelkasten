
2025-10-07 11:23

Source: #paloalto 

Tags: [[Defence]]

Soc also known as computer emergency response teams and computer security incident response teams
# Obj and strategies of soc 

### Planning 
physical safety must be taken into consideration. 
The soc can contain many areas such as the OP room, a war room, and the office of the supervisor 
### Analysis 
Improvement of detection of security incidents through continuous monitoring and analysis of data effectiveness
### Efficiency 
Successful soc leverage threat intelligence and security automatic and orchestration technology 

# Challenges

### Too many tools / alerts 

To block every possible attack vector, many organizations have countless tools, following are the limitations of the current tools
**Prioritization** - don't allow teams to easily prioritize alters for review 
**Full context** - don't provide all the context req for investigation 
**Sophisticated filtering** - separate policies to filter low priority alerts

> Org forwards high lev alerts -> soc analyst pivots to analysis tools -> soc analyst reviews logs and investigates malicious activity 

**Adverse event**
Adverse event is any event that has negative consequence. Malware infection on a system

**Security incident**
A violation or imminent threat of violation of organizational cybersecurity policies, intrusion on a network system, DoS, keylogger 

# Incident Response lifecycle

> Preparation -> detection and analysis -> containment, eradication, and recovery -> post incident activity 
## Frameworks

Based on existing standards, guidelines, and practices for organizations to better manage and reduce their organizational cybersecurity risk. It also encourages comms between internal and external organization stakeholders regarding cybersecurity 
### NIST 

For Improved security posture, following are the 3 major parts 

**Core** 
	provides a set of desired activities to be used in a cybersecurity implementation program to meet w the needs of any size org.
**Implementation tree** 
	helps org to provide an organizational view for cybersecurity risk mgmt and to discuss their risk appetite , budget and priorities 
**Profiles** 
	provides organization a way to strengthen their existing processes or implement new processes. The profiles allow a powerful comms within the organization 
**Plan**
	 each incident response plan has a mission statement, strategies and goals an organizational approach to incident response 
**Review**
	 review each incident with post incident activity so that the organization is prepared for future attacks 
### FedRAMP (Federal risk and Authorization management program)

*Features-* 
**Effective cloud sec**
	Provides a framework for creating and managing repeatable processes for an effective cloud security for the government 

**Marketplace** 
	Has established a mrktplace for cloud services due collaboration across government agencies by use case tactical solutions

**Security baselines** 
	- High w a set of 421 controls
	- Moderate w a set of 325 controls
	- Low w a set of 125 controls
	- LI SaaS w a set of 36 controls
### FISMA (Federal Information Systems Management Act)

Requirements
- Information Systems inventory 
- Information and information systems categorization by their risk level
- Selection of appropriate security controls and assurance requirements
- Risk assessment 
- System security plan 
- Certification and accreditation 
- Continuous monitoring 
### MITRE ATT&CK
ATT&CK frmk is a knowledge base of tactics and techniques used by attacker 

Frmwrk consists of 12 tactics and 332 techniques. 

| Initial access        | Execution        |
| --------------------- | ---------------- |
| Discovery             | Lateral movement |
| Persistence           | Collection       |
| Privileged escalation | Exfiltration     |
| Defence evasion       | C2C              |
| Credential access     | Impact           |

### International organization for standardization (ISO)


| ISO/IEC 27035-1 | principles of incident management                                                             |
| --------------- | --------------------------------------------------------------------------------------------- |
| ISO/IEC 27035-2 | guidelines to plan and prepare for incident response                                          |
| ISO/IEC 27037   | Guidelines for the identification collection acquisition and preservation of digital evidence |
| ISO/IEC 27042   | Guidelines for the analysis and interpretation of digital evidence                            |
| ISO/IEC 27043   | Incident investigation principles and processes                                               |

# SOC goals/mission

SOC needs a clearly defined and well communicated mission statement for the organisations security operations function

##### Elements of soc

Define all elements in the business, people, interfaces, visibility, technology and processes in accordance with the goals

##### Difference between mission and goal

  

Mission

Mission will express the purpose of the soc to all related parties.

Mission is a high level strategy document that represents the main objective, vision, and business drivers of sec ops

  

Goals

Soc goals are the instruments that teams will use to reach the main. Defined goals should address the requirements of an organisations IT

#### Risk

Is the probability of successful exploration of a threat for vulnerability.

##### Identify

Asset inventories are created and asset scores are identified, according the assets function and criticality. Risks are defined for each asset, the owner of the risk is identified

##### Perform RA

- The probability of occuring and impact areas are unique for each organisation.

##### Determine security control

After the risk is identified, analyzed, and classified the control type has to be decided. Control types can be -

- Mitigate

- Transfer

- Accept

- Avoid

Chosen security controls are applied for each asset

##### Check risks to reduce likelihood

Risks are checked against applied controls and defined metrics

##### Monitor the controls

Final step involves critically monitoring and reviewing the effectiveness of the control measures that have been implemented to prevent or minimize the risk

### Incident lifecycle

Effective comms is must

#### Identify

Alerts are processed and classified according to their severity.

##### Alerting

Is how an event is determined to be important enough to become an actionable incident.

Quality of alerts are important for presenting high fidelity alerts to analysts and reducing false positives

##### Building alerting profiles

Content engineering team builds alerting profiles, which identifies the alerts that will be forwarded for investigation.

The interface agreement should identify how the security ops team and threat hunting team will make request for new alerts or modifications to existing alerts

##### Initial research

Is a set of high level process that an organisation uses to begin an investigation into a suspicious alert. The results of the initial research provide context around an Incident to help analyst gather info to triage or escalate the incident

##### Severity triage

Defines the event prioritisation based on impact to the business to help guide the analyst actions through the incident response lifecycle. When automation assigns an initial Severity, the analyst reviews the Severity assignment and validates it against the uniqueness of the organisation

##### Escalation process

Is a set of guidelines that enable the sec ops team to increase the organisations awareness of a potential issue and receive the necessary support. An interface agreement should define what severities require increased awareness from the business

#### Investigate

  

##### Detailed analysis

  

Helps in confidently determining if an incident is " true" Or not. In the event of a false positive feedback should be recorded to be provided to the content engineer to tune alerts or to the security engineering team to update controls in continuous improvement phase

  

Detailed analysis ensures that all relevant information is gathered including -

1. The potential impact of the security incident

2. The affected assets

3. The adversary's objective

4. The potential impact of containment measures

##### Forensics

To define the method an expert witness uses to prepare evidence. The evidence must be repeatable and defensible, the process takes by an expert must not modify any of the original data in any way, and the results must be factual and not tainted by whichever party is funding the work.

##### Telemetry

Is a broad range of activities gathered in real time from a given source.

Telemetry is consistently recorded which makes it more useful than a lot that collects prescribed information only when trigged by a specific event.

  

Telemetry from cloud configuration and network and endpoint activity provides the information necessary to triage and investigate the majority of alerts and incidents

Forensics data provides the information needed to validate breaches.

#### Mitigate

  

A pre approved scenario must be executed. Execution includes breach response actions and change control functions

  

Mitigation Strategies - docs of any actions taken by sec team

  

Pre-approved mitigation scenarios - block an infected laptop from the network to prevent the spread of malware

  

Breach response -

    - identify cross functional stakeholders

    - Assign a timeline of when each stakeholder should become involved and how they will be initially notified

    - define the details of the information to be collected and shared by the security operations team

  

Change control - monitor, document and control the changes that are being made

The docs also should include who can request change, the steps needed to initiate change and any prerequisite or change windows available for the modification

  

Interface agreements - defines how the secops team and surrounding teams interact.

In cases where an interface agreement is not being used or upheld refer to a service level agreement and the change request process and escalation process

  

#### Continuous improvement

  

##### Tuning

Refers to the adjustments orgs make to their alerting procedures for security incidents based on the outcome of security investigation / if they find a better way to deal w something even then tuning is used

The tuning process should define

- Who or what triggers tuning efforts

- Thresholds for those triggers

- A review process for existing alerts

- The steps to request modification to existing alerts

##### Process improvements

The process should include information about who (individual must be a qualified resource knowledgeable in incident response) can update the incidents response processes. Process should define how often each process should be reviewed.

##### Capability improvement

Is rooted in revising prior incidents and asking how future incidents can be better prevented or mitigated

Goal of capability improvement

- Future attack prevention

- Quicker investigation

- Faster identifying and resolution of breaches

- Quicker remediation

### Compliance

  

Can improve their security posture, prevent security breaches, helps analyst meet business objectives, manage risks and meet compliance requirements because compliance standards require different levels of protection and data storage with encryption needs

  

Soc analysts use the following to address compliance needs

- Security information and Event mgmt - eg. Retention of data for long term compliance needs

- Vulnerability mgmt tool - eg. Each systems sec compliance and patch level

- Security policy - eg. Password policy

  

Compliance requirements for soc related functions of the following

##### PCI DSS

  

### Data loss protection

  

Essential features

- Apply protective rigor

- Protect sensitive data

- Minimize data exposure through employee training

  

Challenges are

- diversity of sensitive data

- data is everywhere - orgs run a shadow IT report and discover myriad unsanctioned cloud applications in use

- data is shared and transmitted across many different channels - orgs need a way to monitor the transfer of data both within and outside of the org

- its not ready to identify what data is sensitive - not every 9 digit number is a social security number

  

Data loss prevention solution collects and aggregates 3 types of information to make informed data protection descisions

Content

- Subject matter

- Type of file

- Metadata

Context

- Location

- Device

- Time

Character

- Role

- ID

- Behaviours analytics data