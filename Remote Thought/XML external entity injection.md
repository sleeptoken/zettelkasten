
2024-12-10 14:57

Source: #AOC24 

Tags: [[Injection]]

### Extensible Markup Language (XML)

[[XML]] is a commonly used method to transport and store data in a structured format that is easy to understand.
XML uses `tags` to label and organize information
The key benefit of XML is that it is easily shareable and customizable, allowing you to create your own tags. 

```xml
<people>
	<name>Glitch</name>
	<address>Wareville</address>
</people>
```
### Document Type Definition (DTD)

DTD is a set of **rules** that defines the structure of an XML document.

```dtd
<!DOCTYPE people [ 
	<!ELEMENT people(name, address)> 
	<!ELEMENT name (#PCDATA)>
	<!ELEMENT address (#PCDATA)>
]>
```
### Entities

- Entities in XML are placeholders that allow the insertion of large chunks of data or referencing internal or external files. 
- Entities can be defined internally within the XML document or externally, referencing data from an outside source.

```
<!DOCTYPE people [
	<!ENTITY ext SYSTEM "http://tryhackme.com/robots.txt"> 
]>
<people>
	<name>Glitch</name> 
	<address>&ext;</address>
</people>
```
### References
DAY 5 