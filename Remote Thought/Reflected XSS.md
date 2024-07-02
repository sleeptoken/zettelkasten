
2024-06-05 12:43

Source: #tryhackme #web

Tags: [[XSS]]  

- **Reflected XSS**: This attack relies on the user-controlled input reflected to the user. For instance, if you search for a particular term and the resulting page displays the term you searched for (**_reflected_**), the attacker would try to embed a malicious script within the search term. This attack happens often via a **crafted URL** or **form submission**

[[PHP]]
`$_GET` is a PHP array containing values from the URL query string
The PHP function `htmlspecialchars()` converts special characters to HTML entities.
Before reflecting the user input, using the above php function will replace certain characters with predefined set of chars (like encoding)

[[JavaScript]] (Node.js)
using the `sanitizeHtml()` from the `sanitize-html` library. This function removes unsafe elements and attributes. This includes removing script tags, among other elements that could be used for malicious purposes. 
using the `escapeHtml()` function which aims to escape characters such as `<`, `>`, `&`, `"`, and `'`

[[py]] (Flask)
- `request.args.get()` is used to access query string parameters from the request URL.
- `request.args` contains all the query string parameters in a dictionary-like object
 using the `escape()` function from the `html` module. Note that the `html.escape()` function in Flask is actually an alias for `markupsafe.escape()`. They both come from the Werkzeug library and serve the same purpose: escaping unsafe characters in strings.

[[ASP.NET]] C#.
`Request.QueryString`, which returns a collection of associated string keys and values
`HttpUtility.HtmlEncode()` which converts various characters, such as `<`, `>`, and `&`, into their respective HTML entity encoding.

### References
