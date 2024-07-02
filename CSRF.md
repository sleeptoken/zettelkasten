
2024-06-30 13:07

Source: #portswigger #web 

Tags:  [[API]]

It allows an attacker to partly circumvent the `same origin policy`, which is designed to prevent different websites from interfering with each other. 

key conditions must be in place: 
1. A relevant action. There is an action within the application that the attacker has a reason to induce.
2. Cookie-based session handling. Performing the action involves issuing one or more HTTP requests, and the application relies solely on session cookies to identify the user who has made the requests. There is no other mechanism in place for tracking sessions or validating user requests.
3. No unpredictable request parameters. The requests that perform the action do not contain any parameters whose values the attacker cannot determine or guess. For example, when causing a user to change their password, the function is not vulnerable if an attacker needs to know the value of the existing password. 

 Although CSRF is normally described in relation to cookie-based session handling, it also arises in other contexts where the application automatically adds some user credentials to requests, such as HTTP Basic authentication and certificate-based authentication. 




### References
https://portswigger.net/web-security/learning-paths/csrf