
2024-07-28 19:45

Source: #portswigger #web 

Tags: [[Injection]]
#### There are two different types of NoSQL injection:

- **Syntax injection** - This occurs when you can break the NoSQL query syntax, enabling you to inject your own payload. The methodology is similar to that used in SQL injection
- **Operator injection** - This occurs when you can use NoSQL query operators to manipulate queries.
## NoSQL syntax injection

- You can potentially detect NoSQL injection vulnerabilities by attempting to break the query syntax. To do this, systematically test each input by submitting fuzz strings and special characters that trigger a database error or some other detectable behavior if they're not adequately sanitized or filtered by the application.
- If you know the API language of the target database, use special characters and fuzz strings that are relevant to that language. Otherwise, use a variety of fuzz strings to target multiple API languages.

NoSQL injection vulnerabilities can occur in a variety of contexts, and you need to adapt your fuzz strings accordingly. Otherwise, you may simply trigger validation errors that mean the application never executes your query. 

 To determine which characters are interpreted as syntax by the application, you can inject individual characters. 

Testing
1. Determining which characters are processed
	For example, you could submit `'`, which results in the following MongoDB query:
	`this.category == '''`
	
	If this causes a change from the original response, this may indicate that the `'` character has broken the query syntax and caused a syntax error. You can confirm this by submitting a valid query string in the input, for example by escaping the quote:
	`this.category == '\''`
	
	If this doesn't cause a syntax error, this may mean that the application is vulnerable to an injection attack. 
2. Confirming Conditional behavior - Determine whether you can influence boolean conditions using NoSQL syntax.
	- To test this, send two requests, one with a false condition and one with a true condition. For example you could use the conditional statements `' && 0 && 'x` and `' && 1 && 'x` 
	- If the application behaves differently, this suggests that the false condition impacts the query logic, but the true condition doesn't. This indicates that injecting this style of syntax impacts a server-side query. 
3. Overriding existing conditions - 



### References
https://portswigger.net/web-security/learning-paths/nosql-injection