
2025-01-24 10:07

Source: 

Tags: [[regex]]

Regex evaluation happening in the backend sever
example : 
```
>{ version: “3.111”, metadata: { name: “test+0”, type: “pipe”, repo: “sample” } }
```

When appending a “+0” to the value of ‘name’ parameter, the server responded with ‘422 Unprocessable Entity’ and surprisingly, in the response body the application revealed a message which said something like below:

```
Status: Failure; <some_except... haracter (e.g. ‘example.com’, regex used for validation is ‘[a-z0–9]([-a-z0–9]*[a-z0–9])?(.[a-z0–9]([-a-z0–9]*[a-z0–9])?)*’),field:metadata.name}]},code:422}
```

The next thing you check if there are any validation on the length/size of the parameter value. length parameters will have more response time. further use turbo intruder to complete DOS attack.
### References
[Regular expression Denial of Service - ReDoS | OWASP Foundation](https://owasp.org/www-community/attacks/Regular_expression_Denial_of_Service_-_ReDoS)
Bug Bounty - https://medium.com/@ashkrypt/denial-of-service-dos-by-regex-205536c8dcd0