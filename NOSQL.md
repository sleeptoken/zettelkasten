https://unstop.com/competitions/1024247/register
2024-07-28 19:45

Source: #portswigger #web 

Tags: [[Injection]]

> [!NOTE]
>  Remember to URL Encode -> Ctrl + U

#### There are two different types of NoSQL injection:

- **Syntax injection** - This occurs when you can break the NoSQL query syntax, enabling you to inject your own payload. The methodology is similar to that used in SQL injection
- **Operator injection** - This occurs when you can use NoSQL query operators to manipulate queries.
## NoSQL syntax injection

- You can potentially detect NoSQL injection vulnerabilities by attempting to break the query syntax. To do this, systematically test each input by submitting fuzz strings and special characters that trigger a database error or some other detectable behavior if they're not adequately sanitized or filtered by the application.
- If you know the API language of the target database, use special characters and fuzz strings that are relevant to that language. Otherwise, use a variety of fuzz strings to target multiple API languages.

NoSQL injection vulnerabilities can occur in a variety of contexts, and you need to adapt your fuzz strings accordingly. Otherwise, you may simply trigger validation errors that mean the application never executes your query. 

 To determine which characters are interpreted as syntax by the application, you can inject individual characters. 
 
>  Testing
> 
> 1. Determining which characters are processed
> 	For example, you could submit `'`, which results in the following MongoDB query:
> 	`this.category == '''`
> 	
> 	If this causes a change from the original response, this may indicate that the `'` character has broken the query syntax and caused a syntax error. You can confirm this by submitting a valid query string in the input, for example by escaping the quote:
> 	`this.category == '\''`
> 	
> 	If this doesn't cause a syntax error, this may mean that the application is vulnerable to an injection attack.
> 
> 2. Confirming Conditional behavior - Determine whether you can influence boolean conditions using NoSQL syntax.
> 	- To test this, send two requests, one with a false condition and one with a true condition. For example you could use the conditional statements `' && 0 && 'x` and `' && 1 && 'x` 
> 	- If the application behaves differently, this suggests that the false condition impacts the query logic, but the true condition doesn't. This indicates that injecting this style of syntax impacts a server-side query. 
> 
> 3. Overriding existing conditions - 
> 	 For example, you can inject a JavaScript condition that always evaluates to true, such as `'||1||'`
> 	 
> 	As the injected condition is always true, the modified query returns all items. This enables you to view all the products in any category, including hidden or unknown categories.
> 	
> 	*Take care when injecting a condition that always evaluates to true into a NoSQL query. Although this may be harmless in the initial context you're injecting into, it's common for applications to use data from a single request in multiple different queries. If an application uses it when updating or deleting data, for example, this can result in accidental data loss.
> 	
> 	You could also add a `null` character after the category value. `MongoDB` may ignore all characters after a `null` character. This means that any additional conditions on the `MongoDB` query are ignored.
#### Lab

1. on submitting a `'` character in the category parameter. Notice that this causes a **JavaScript syntax error**. This may indicate that the user input was not filtered or sanitized correctly.

2. Submit a valid JavaScript payload in the value of the category query parameter. eg -> `Gifts'+'`
	 Make sure to URL-encode the payload. Notice that it doesn't cause a syntax error. This indicates that a form of server-side injection may be occurring.

```
Insert a false condition in the category parameter
Gifts' && 0 && 'x

Insert a true condition in the category parameter
Gifts' && 1 && 'x
```
3. Make sure to URL-encode the payload. Notice that products in the Gifts category are retrieved.

4. Submit a boolean condition that always evaluates to true in the category parameter. For example:
`Gifts'||1||'`

## Exploiting syntax injection to extract data

In many NoSQL databases, some query operators or functions can run limited JavaScript code, such as MongoDB's `$where` operator and `mapReduce()` function.
### Exfiltrating data in MongoDB

Consider a vulnerable application that allows users to look up other registered usernames and displays their role. This triggers a request to the URL:
```http
https://insecure-website.com/user/lookup?username=admin
```

This results in the following NoSQL query of the users collection:
```sql
{"$where":"this.username == 'admin'"}
```

As the query uses the `$where` operator, you can attempt to inject JavaScript functions into this query so that it returns sensitive data. For example, you could send the following payload:
```sql
admin' && this.password[0] == 'a' || 'a'=='b
```

This returns the first character of the user's password string, enabling you to extract the password character by character.

You could also use the JavaScript `match()` function to extract information. For example, the following payload enables you to identify whether the password contains digits:
```sql
admin' && this.password.match(/\d/) || 'a'=='b 
```

### Identifying field names

Because MongoDB handles semi-structured data that doesn't require a fixed schema, you may need to identify valid fields in the collection before you can extract data using JavaScript injection.

For example, to identify whether the MongoDB database contains a password field, you could submit the following payload:
```
https://insecure-website.com/user/lookup?username=admin'+%26%26+this.password!%3d'
```

Send the payload again for an existing field and for a field that doesn't exist. In this example, you know that the username field exists, so you could send the following payloads:
```sql
admin' && this.username!=' 
admin' && this.foo!='
```

If the password field exists, you'd expect the response to be identical to the response for the existing field (username), but different to the response for the field that doesn't exist (foo).

You can alternatively use NoSQL operator injection to extract field names character by character. This enables you to identify field names without having to guess or perform a dictionary attack.

### *Lab*

The user lookup functionality for this lab is powered by a MongoDB NoSQL database.








## NoSQL operator injection

in JSON messages, you can insert query operators as nested objects. For example, `{"username":"wiener"}` becomes `{"username":{"$ne":"invalid"}}`

For URL-based inputs, you can insert query operators via URL parameters. For example,` username=wiener` becomes `username[$ne]=invalid`. If this doesn't work, you can try the following:

>**Testing** 
    1. Convert the request method from `GET` to `POST`.
    2. Change the `Content-Type` header to `application/json`.
    3. Add `JSON` to the message body.
    4. Inject query operators in the `JSON`.

### Detecting operator injection in MongoDB

Test each input with a range of operators. For example, to test whether the username input processes the query operator, you could try the following injection:
```json
{"username":{"$ne":"invalid"},"password":{"peter"}}
```

If the `$ne` operator is applied, this queries all users where the username is not equal to invalid.

If both the username and password inputs process the operator, it may be possible to bypass authentication using the following payload:
```json
{"username":{"$ne":"invalid"},"password":{"$ne":"invalid"}}
```

This query returns all login credentials where both the username and password are not equal to invalid. As a result, you're logged into the application as the first user in the collection.

To target an account, you can construct a payload that includes a known username, or a username that you've guessed. For example:
```json
{"username":{"$in":["admin","administrator","superadmin"]},"password":{"$ne":""}}
```
#### *Lab*

trying wiener as username which we know is a valid username and password as `"$ne" : "peter"` which is a valid password but we are passing it with not equal to we get a `200 OK` . This confirms that password parameter is vulnerable 

- if we get a `302` error then that means the condition in the parameters is true 
- if we put administrator as username we get 200 that means user with that name does not exist

if you don't find the username by guessing then use [[regex]]
```json
"username":{
	"$regex" : "^a"
}
```
`^a `means anything that starts with `a`  -  we find a username named `admin7543`

copy the cookie and replace you current cookies with the new ones 



### References
https://portswigger.net/web-security/learning-paths/nosql-injection