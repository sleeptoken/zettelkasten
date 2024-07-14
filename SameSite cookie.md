
2024-07-14 22:20

Source: #portswigger 

Tags: [[CSRF]]

`SameSite` is a browser security mechanism that determines when a website's cookies are included in requests originating from other websites.

Since 2021, Chrome applies Lax SameSite restrictions by default if the website that issues the cookie doesn't explicitly set its own restriction level. This is a proposed standard, and we expect other major browsers to adopt this behavior in the future. As a result, it's essential to have solid grasp of how these restrictions work, as well as how they can potentially be bypassed, in order to thoroughly test for cross-site attack vectors. 











### References
