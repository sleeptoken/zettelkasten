
2025-11-11 16:18

Tags: [[Automation XSIAM]]
 
XQL is designed for use with XML data in Cortex. It allows users to query and manipulate XML data in a flexible and extensible manner that makes querying easy and intuitive. used for both reactive tasks, like doing case investigations, and proactive tasks, like creating detection rules.

Query Builder 
allowing you to search for indicators of compromise and suspicious patterns within data sources. 

USAGE OF QUERY BUILDER 
Investigate any lead quickly
Expose the root cause of an issue
Perform damage assessment
Hunt for threats from data sources
Build complex queries for entities and entity attributes

By using the query editor effectively, you can streamline your investigations and pinpoint issues more quickly

##### Building XQL

Every XQL query is built using a series of stages. A stage as a step in a process where data is refined and filtered until you get the results you need. A query can have one or more stages, depending on the complexity of the analysis you're performing.

Run option blocks you from going to the other pages of the management console as it executes the commands 

Every XQL dataset query begins by identifying a data source that the query will run against.

Primary Dataset
xdr_data: Raw EDR and stitched events

Other Datasets
panw_xdrc_raw: XDR Collectors logs
vpn_logs: VPN activity, e.g., GlobalProtect
aws_s3_raw: AWS CloudTrail/CloudWatch logs
google_cloud_logging_raw: GCP logs
microsoft_windows_raw: Windows event logs

##### Preset Datasets

Presets are subsets of the larger dataset available in Cortex. Preset can be used in conjunction with datasets and data models. Presets are tailored to meet the requirements of common security use cases and scenarios encountered by organizations.

there are 2 types of presets 
Unlike regular presets, which focus on organizing fields based on general categories of security data, story presets emphasize storytelling by presenting data in a narrative format that highlights the sequence of events, relationships between entities, and potential cause-and-effect scenarios.

There are two presets available: authentication_story and network_story. These two presets are created from the respective xdr_data fields around stories.

Field Groupings
Each preset comprises a carefully selected grouping of fields that are most useful for analyzing the designated area of network or endpoint activity. These fields may include attributes such as source and destination IP addresses, user identities, and more, depending on the specific focus of the preset. By organizing fields into logical groupings within presets, Cortex simplifies the analysis process and provides analysts with quick access to the relevant data points needed to investigate security cases or anomalies effectively.

Predefined Fields
Since presets contain a predefined set of fields relevant to a particular analysis task, queries executed using presets can run more efficiently compared to queries that scan the entire dataset.

Query Performance Optimization
By leveraging presets, analysts can optimize their query performance, reduce query execution times, and focus their analysis efforts on the most relevant data elements. This efficiency enhancement enables security teams to expedite their investigation processes and respond promptly to emerging security threats. 

2 Types of presets 

Regular Presets
Regular presets are predefined configurations that organize fields relevant to specific areas of network or endpoint activity. Focus on providing a comprehensive set of fields necessary for analyzing common security use cases

Contain Attributes
Regular presets typically include essential attributes such as source and destination IP addresses, user identities, timestamps, event types, severity levels, and other relevant metadata associated with security events. 

Story Presets
Story presets are specialized configurations that organize fields into common schemas or narratives known as "stories." These stories correlate related security events and contextual information to provide a cohesive view of specific security cases, attack patterns, or behavioral trends observed within the network or endpoint environment.

Unlike regular presets, which focus on organizing fields based on general categories of security data, story presets emphasize storytelling by presenting data in a narrative format that highlights the sequence of events, relationships between entities, and potential cause-and-effect scenarios.

###### XQL use cases

Case Response (Reactive)
Build Custom Widgets
Detection and Correlation Rules
Parsing Rules
Threat Hunting (Proactive)

### Syntax

Stages
Stages perform certain operations in evaluating queries. Commonly used stages include dataset, stages are separated by pipes (|). 
```
fields, filters, join, and sort.
```

Functions
Some stages can call functions to convert the data to a desired format. 
The filter and alter stages are the two stages that can use functions for data transformations.
```
| alter timepart = extract_time(current_time(), "HOUR")
```

Operators
Operators in programming languages are function-like constructs but differ syntactically and semantically. 
XQL queries can use operators usually to relate some data (variables and/or values). XQL operators are comparison operators, Boolean operators, array and range operators, and a tagging operator. For example, "in" in the example below is a range operator that returns a Boolean value.

```
| filter action_local_port in (1024,9999)
```

XQL schema provides information about a particular dataset, such as a description of all fields, while XQL syntax deals with grammar or language elements. 

#### Datasets
 ---- dataset = xdr_data 
If you omit the dataset stage, the XQL query compiler will not generate an error. Instead, the XQL query engine will run the query against the default dataset. This is called Implicit dataset 

If u declare a dataset it is called explicit dataset 

```
dataset = xdr_data
| fields actor_process_image_name as actProcess 

---

fields actor_process_image_name as actProcess

both result in the same output 
```

#### Line break 

The XQL query compiler ignores line breaks, enabling you to format your query using line breaks for improved readability.

Note the differences between line breaks and word wrap (also known as soft word wrap). When you press Enter, Windows inserts two characters, a carriage return (CR) and a line feed (LF), to indicate a line break. On the other hand, UNIX/Linux systems use only LF to represent a line break.

#### Command Nature
same as in C/C++ and Java.

Line comments
Insert double slashes (//) 

Block comments 
span multiple lines using /* and */.

XQL commands are case insensitive 

In XQL, string comparisons are case sensitive by default. 

You can make string comparisons case insensitive using the config stage.

```
config case_sensitive = false 
| filter actor_process_image_name = "POWERshell.EXE"
```

#### Accessing the XQL Development Environment

From the Query Builder
go to Incident Response > Query Builder > XQL.

From the Query Center
go to Query Center and then click the +New XQL Query button.

Using the Browser
type the relative path /xql in your web browser. 

Using the Quick Launcher
click the Quick Launcher, type /XQL, press Enter and click the path that pops up.  

##### Options for Running Queries

Run
With Run, you can immediately run your query as a foreground task

Schedule a Query
You can schedule two types of queries: periodic and non-periodic. A non-periodic scheduled query runs once at a single point in time. A periodic scheduled query runs periodically at a specified frequency.

Save As
With Save as, you can save your XQL query for later use as a behavioral indicators of compromise (BIOC) or Correlation rule.

#### Stages in a Query

Parameters
All the stages require some parameters to operate. The examples below show parameter usages in XQL. The examples below show parameter usages in XQL. 

Dataset
The dataset stage needs an assignment.

```
dataset = xdr_data 
```

Filter
The filter stage requires a Boolean expression to evaluate at runtime.

```
filter actor_process_image_name = "powershell.exe" 
```

Fields
The fields stage requires a comma-separated list of fields.

```
fields event_type, event_sub_type as SUBTYPE
```

Limit
The limit stage requires an integer.
`limit 4`

##### Config Stage (optional)

It should be the first stage in a query.

Compile-Time Case-Sensitive
The compile-time, case-sensitive parameter takes a Boolean (true or false) value and forces string comparisons that are either case-sensitive or case-insensitive.

Timeframe
The timeframe parameter is a runtime parameter that sets the timeframe for query evaluation, meaning the XQL query engine only considers logs created within this range. Below cmds are for fine tuning
```
config case_sensitive = true | false
---
config timeframe = <number><time unit
```

To change the default dataset, navigate to CONFIGURATIONS > Data Management > Dataset Management section.

dataset = <dataset name>
dataset in (comma-separated list of datasets)

The target stage saves the result set in a target dataset. You can download only lookup types in the Cortex XSIAM management console under Dataset Management.


##### Target stage 

If used, the target stage must be the last in a query. In the target stage, you specify the data elements to extract, analyze, or manipulate.
```
target type=dataset|lookup append=true|false <dataset name>
```

##### Fields(plural) stage & Filter(singular) stage

fields [-] <field1> [as <name1>], <field2> [as <name2>], ...

Example query includes two field stages. The first field stage is ignored because the second field overrides it. The second field stage for field exclusion is ignored because a hyphen is not specified in the stage.

Multiple filter stages can be used in an XQL query, each separated by a pipe (|), the regular stage separator symbol. The net effect is that the AND of all Boolean values is evaluated for the filter stages used in the query.

the filter stage specifies the rows of the table, while the fields stage specifies the columns of the table. 

##### Alter stage 

The alter stage assigns values to fields for use in later stages In the alter stage, you can modify or transform the data in your query results. It involves altering or modifying the queried data to derive new insights or manipulate the output.

The alter stage does not modify the backend data. Instead, the XQL query engine runs on copies of the backend data in memory
`alter <variable name> = <value>`

#### Comp stage 

The comp stage performs aggregation functions in the result set.

While reading this stage, first locate the keyword "by" that separates the two field lists. The second list after "by" is used to group the rows of the result table. Each group can be uniquely identified based on the <field1[,<field2>...] combination.
```
comp <function> [as <alias>] by <field1>[,<field2>..]
```

Limit
Sets the maximum number of entities in the result set; cardinality of the set.
•Limit <number>

Sort
Sets the sort order of the entities in the result set—for example, asc, desc.
•sort asc|desc <field1>[, asc|desc <field2>...]

Dedup
Removes duplicate values in the specified fields in the result set.
•dedup <field1>[,<field2>, ...] by asc | desc <field>

Field Order
The XQL query compiler can detect incorrect stage placements and display syntax errors.
Stage order is important in XQL because the output of one stage can serve as the input of the next stage.

`getrole stage` enriches events with specific roles associated with usernames or endpoints. The results are displayed in a new column called asset_roles. This stage is unsupported in BIOCs and real-time Correlation Rules

#### Functions 

The general syntax for XQL functions is as follows:

```
<return-value> = func_name([<comma-separated-argument-list>])
```

Advanced XQL Functions

Data Manipulation and Filtering Functions

functions such as avg(), count(), first(), and last().
array_all() function: Returns true/false whether or not all elements in the array are matching the condition
The array_any() function: Returns true/false whether or not at least 1 element is matching the condition

Windowcomp Functions
The windowcomp stage precedes functions calculating statistics. The functions compute values over a group of rows and returns a single result for each row for all records that contain matching values for the fields identified using a combination of the by clause, sort, and range. Some of the windowcomp functions are: rank, first_value, stddev_sample, avg, and median.

Function Syntax Symbols

Angle Brackets <>
These are often used to indicate a placeholder for the actual arguments you will provide when using the function.

Square Bracket [ ]
This symbol indicates an optional element; anything enclosed in [ ] is optional.

Ellipsis …
The ellipsis symbol indicates the function can take a varying number of arguments separated by commas starting at this ellipsis argument position.

Function Syntax
```
alter ret = arraycreate(<arg1> [, <arg2> [, ...]])
```

In XQL, "true" and true are different.

Here the syntax in “ax1->user{}” is called "syntactic sugar" format. Syntactic sugar is syntax within a programming language that is designed to make things easier to read or to express. 

The "ax1->user{}" is in syntactic sugar format.

```
dataset = xdr_data 
| alter ax1 = object_create("user","Sar", "pass", "Mer") 
| alter ax3 = ax1->user{} 
| fields ax* | limit 1
```

XQL supports RE2 for regular-expression processing. RE2 is a software library for regular expressions via a finite-state machine using automata theory, in contrast to almost all other regular expression libraries, which use backtracking implementations. It provides a C++ interface.

#### JSON Obj manipulation

There are two sources for JSON (JavaScript Object Notation) objects in XQL queries:
JSON fields in digested datasets (collected logs) 
```
dataset = xdr_data 
| filter agent_interface_map != null 
| fields agent_interface_map
```
JSON-type variables explicitly created in XQL queries.

Some XQL functions can also return JSON type variables. These functions are object_create(), json_extract(), and to_json_string(). 

```
alter jsonObj1 = object_create("user", "SAR", "pass", "MER")
```
Before XDR_DATA JSON fields can be used as JSON type variables in XQL, they must be converted using to_json_string().

Ways to access JSON in XQL

JSON processing functions
The main operation in accessing a JSON object in XQL is to extract certain parts of the object
    json_extract_scalar()
    json_extract_array()
    json_extract()
    object_create() 
    to_json_string()

Syntactic Sugar Format

There are three syntactic sugar notations, one for each type of JSON part accessed: string, array, and object types.
Square brackets ([ ]) are used for JSON arrays, while curly braces ({ }) are for JSON objects. 

#### JSONPath
JSONPath is a set of expressions defining paths to JSON objects' elements. JSONPath is a language with a defined grammar and is very similar to XPath of XML.

JSONPath offers two different types of notation: dot notation with the dot operator (.) and bracket notation with square brackets ([ ]). The root element (also called the root node) is denoted by a dollar sign ($). 

Dot Notation 
A path is created in dot (.) notation by separating the element names with a dot (.) starting from the root element denoted by $. The basic syntax is $.parentElement.element...childElement;

Bracket  Notation 
In brackets ([‘ ‘]) notation, a path is created by enclosing the element names in square brackets starting from the root element $. Element names are quoted in the brackets. There is no symbol used to separate two consecutive square brackets.
The basic syntax is: $[‘parentElement’][‘element’]…[‘childElement’]. 

XQL supports JSONPath notations in JSON extract functions. The query has multiple alter stages, each with a call to json_extract_scalar(). 

JSON Extract funcitons

Scalar
Extracting scalar values includes identifying and isolating the desired scalar values from complex data structures like arrays, objects, or JSON.
json_extract_scalar(jsonObj, jsonScalarPath) returning string

Array
Extracting arrays allows you to retrieve specific elements or values from arrays within your data. You can extract the necessary information from the array by specifying the index or condition.
json_extract_array(jsonObj, jsonArrayPath) returning array of strings

JSON Object
JSON child objects are nested within other objects, forming a hierarchical structure. XQL allows you to query data from JSON child objects to retrieve specific nested data within a JSON structure.
json_extract(jsonObj, jsonObjPath) returning JSON object

Use the json_extract_array() function to access JSON elements that are arrays of some scalar type

Avoid using json_extract with JSON array paths. The XQL engine can still run such queries, but extracted values are not useful.

Syntactic Sugar Format for Scalars

This format specifies the JSON variable/field and path to child element, without the $ root element symbol. 
The XQL sugar format for referencing scalars in JSON objects is jsonObj->jsonScalarPath. 

Syntactic Sugar Format for Arrays

The XQL sugar format for referencing arrays in JSON objects is jsonObj->jsonArrayPath[]. The format is: jsonObj->jsonArrayPath[]. Referencing a variable using jsonObj->jsonArrayPath[] is equivalent to calling the function json_extract_array(jsonObj, jsonArrayPath).

Syntactic Sugar Format for JSON Objects

The XQL sugar format for referencing child JSON objects is jsonObj->jsonObjPath{}
Referencing a variable using jsonObj->jsonScalarPath is equivalent to calling the function json_extract(jsonObj, jsonObjPath).

Dataset Schemas 
A schema is data about a dataset, which is stored in another dataset. A schema dataset basically provides a description of all the fields for a given dataset.

XSIAM datasets are non-relational (NoSQL) databases that store data in nontabular forms. 

Data types in alter stage 

Assignment
alter <variable> = <expression>

Comparison
filter <expression_1> = <expression_2>

You cannot use expressions or values on the left-hand side (LHS) of an assignment in alter stages. For comparisons in filter stages, a value is always associated with a data type, and both values on the LHS and right-hand side (RHS) of a comparison must match in their data type.

Note: The fields that you change using the alter stage can be existing or nonexisting. Existing fields are updated, whereas nonexisting fields are newly created and added to the intermediate result set.

All XQL operations occur in intermediate result sets that are in-memory copies of the datasets. The updated and newly created fields are displayed in the result sets of the executed queries.

> XQL is a data-access language, not a data-manipulation language. 

Filter stage comparison 
Note: To check for any Boolean expression, you can use the if() function

Autocomplete Feature for Enum Types
An enum type (also called an enumerated or enumeration type) is a data type that consists of a set of predefined values in dataset schemas. 

Note that the enumeration type can not be contained in an assignment in alter stages; this type is only for comparisons in filter stages.

##### Comparison and Boolean Operators

Cortex XQL organizes operators into different groups

In XQL, the equality operator (=) is overloaded based on the stage.

Equality in the Alter Stage
When used in the alter stage, the equal sign functions as an assignment operator.

Equality with the Filter Stage
When used in the filter stage, the equal sign functions as a comparison operator that returns either true or false.

##### Range Op
Range operators are Boolean-valued and return true or false depending on the membership status.

XQL includes two primary range operators: “in” and “incidr”. 
Both operators search for an item in the list specified in the right operand based on the item specified in the left operand. 

In
To use the "in" operator, you need to specify a list of comma-separated items enclosed in parentheses, for example, (item1, item2, item3).

Incidr
The "incidr" operator is a special operator specific to IP address lists. It checks if an IPv4 address exists in the address list given in Classless Inter-Domain Routing (CIDR) notation. Therefore, the right operand of "incidr" is an IPv4 address range, such as "192.168.10.0/24" in CIDR format.

Incidr6
Tests if an IPv6 address is in CIDR range or not.

Primary XQL String Operators

String operators facilitate efficient manipulation and comparison of string values in queries, including pattern matching and information extraction. 

Contains and Not Contains
The "~=" operator, also known as the regular expression matching operator

Filter is the primary XQL stage where XQL operators are used

"=" XQL operator has two different functions that differ depending on the stage in which it is used

You can use the plus (+) symbol as a string concatenation operator in XQL, for example:

```
alter str1 = "XSIAM" + " is " + "fun."
```
There is no arithmetic operator in XQL. This means that the plus sign is not defined over the numbers. The following generates a runtime error:
```
alter num1 = 4 + 5 // Error: Expected string but received number.
```

#### Query Results in Tables

By default, query results are displayed on the Table display mode in tabular form, in rows and columns known as the results table.

The only difference between Table and Advanced is that the Advanced mode displays query results in two columns, namely _TIME and EVENT. In Advanced, the rows of the result table are converted to a log-like format. 

The Graph display mode lets you view query results as graphs; several chart types are available for use such as column, pie, and map

A histogram is simply a table of three columns: Value, Count, and Percentage. These histogram columns are the same for all results table fields and do not change. 

The supported log formats are RAW, JSON, and TREE

Different values of a field can be unpredictable; therefore, the histogram table displays the first 10 values of the selected field, sorted by count (or equivalently by percentage). 

#### Join Stage Basics

The join stage combines data from multiple sources based on a specified condition or key.

The terminology used in XQL mainly comes from SQL jargon. When using join stages, options such as "type is left" or "conflict_strategy is right" are available. In SQL, the term "left" refers to the main result set, while "right" refers to the join result set created by the join stage itself by executing its own query.

Clauses are building blocks or constituent components of stages
Synopsis of XQL Join Stage
There are five clauses in XQL join. 

```
Join [type = inner |left | right]
[conflict_strategy = both | left | right]
(<inner xql query>)
as <execution_name>
<boolean_expr>
```

Join Type
The join type specifies how two result sets are combined. There are three "type" options: inner, left, and right. The type setting can affect how the records of the two sets are combined.

The XQL join syntax has required and optional clauses. A minimal join stage should include a query for the join, an as clause, and a condition clause (also known as the join predicate).

Name Conflict Strategy
In addition to rows (records), the join stage combines the columns (fields) of the two result sets. However, result sets can have fields with the same name, possibly with different values; therefore, a name conflict may occur during the merge. The conflict_strategy clause is to resolve the name conflict in different ways. Example

if both sets have fields with name FLD:
Left: Combined set shows only FLD of the main result set.
Right: Combined set shows only FLD of the join result set. The default conflict_strategy is right.
Both: Both fields are copied to the combined set, which shows the FLD of the main set as is and the FLD of the join set under the name JOIND_FLD.


Join Query 
This is any valid XQL query enclosed in parentheses () whose execution result will be combined with the main query.

Join Result Set Alias
The result set alias is a name that refers to the join result set (also known as the right set) in the remainder of the main query flow. An alias for the right set is required to separate its fields from those of the left set, which is the result set of the main query.

Join Match Condition
The join match condition is a Boolean expression that specifies how the records of the two sets are matched. A Boolean expression here is just like those specified in a filter stage. 

Considering A and B as two result tables in XQL, the Cartesian product is created by matching all rows of table A with each row of table B

Joins in Query Languages (QL) are reduced Cartesian product sets. Reducing here means deselecting the unmatched records in the Cartesian product so that they are not included in the combined result set.

Difference bw QL Join types 
QLs such as XQL and SQL have many communalities. In SQL, the Cartesian product of two tables is known as a Cartesian join, also known as a CROSS JOIN. (It is different from a FULL JOIN.) There is no join type like a Cartesian join or cross-join in XQL, but the XQL join stage can generate the Cartesian join simply by using a hardcoded true in the join condition.

Inner type join (default)
An inner type join returns all records that match values in both left and right result sets. 

Left type join 
A left type join returns all records in the left set, as well as matching records in the right set.

Right type join 
A right type join returns all records in the right set, as well as matching records in the left table

Cartesian Join
If two tables have n and m rows, the Cartesian join has n*m rows. What makes a Cartesian join different from a typical join is the join condition, which is made up of Boolean expressions used in the filter stages. In a Cartesian join, the join condition is hardcoded as "true."

### Parsing rules

The purpose of parsing rules is to remove irrelevant parts from incoming raw logs.

To work with parsing rules in the Cortex XSIAM management console, a user needs the highest possible authorization levels
The purpose is adding or normalizing timestamps when the source is not sending them. & is stripping invalid characters from logs

The subset of XQL used in parsing rules is called XQLp, which stands for XQL for parsing
The XQL stages that can be used in parsing rules are alter, fields, filter, join, and call.
These sections are INGEST, COLLECT, CONST, EXTEND, and RULE; where INGEST is the only required section.

##### Ingestion Flow Components
You can attach multiple Broker VM instances and XDR Collector instances to the same single XSIAM instance. Multiple instances of the same type are required for load balancing and avoiding having a single point of failure.

#### Call commands

The call stage command initiates a query from the Query Library, effectively incorporating saved queries into active ones.

The call command can be used in conjunction with the join and union stages to merge data from different datasets. However, the join stage is only supported in the CONST, INGEST, and RULE sections, not in the COLLECT section. Furthermore, the call command cannot reference a RULE section from Default Rules within the User Defined Rules section.

Call Stage Syntax
```
call "<name of predefined query>" [<param_name1> = <value1> <param_name2> = <value2>....]
```

> Essentially, the call command serves as a dynamic lookup within other queries.

Advantage of call stage 

Flexibility
The call stage allows you to insert other queries with varying time frames into your current query, providing flexibility that is not commonly associated with XQL.

Object-Oriented Coding
The call stage can be used in a manner similar to object-oriented coding. By inserting a series of call stages into a simpler query, you can execute more complex queries without having to modify the saved queries.

### Graph search 

	


### References