
2024-07-14 22:20

Source: #portswigger 

Tags: [[CSRF]]

`SameSite` is a browser security mechanism that determines when a website's cookies are included in requests originating from other websites.

Since 2021, Chrome applies `Lax` `SameSite` restrictions by default if the website that issues the cookie doesn't explicitly set its own restriction level. 

You may come across the term "effective top-level domain" (`eTLD`). This is just a way of accounting for the reserved multipart suffixes that are treated as top-level domains in practice, such as `.co.uk.` 

If the website issuing the cookie doesn't explicitly set a `SameSite` attribute, Chrome automatically applies `Lax` restrictions by default. This means that the cookie is only sent in cross-site requests that meet specific criteria, even though the developers never configured this behavior.







### References
