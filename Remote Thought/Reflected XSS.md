
2024-06-05 12:43

Source: #tryhackme 

Tags: [[XSS]] [[web]]

- **Reflected XSS**: This attack relies on the user-controlled input reflected to the user. For instance, if you search for a particular term and the resulting page displays the term you searched for (**_reflected_**), the attacker would try to embed a malicious script within the search term. This attack happens often via a **crafted URL** or **form submission**

[[PHP]]
`$_GET` is a PHP array containing values from the URL query string
The PHP function `htmlspecialchars()` converts special characters to HTML entities.
Before reflecting the user input, using the above php function will replace certain characters with predefined set of chars (like encoding)

[[JavaScript]] (Node.js)

### References
