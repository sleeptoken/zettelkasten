
2024-07-09 11:15

Source: #web 

Tags: [[API]] 

| Authentication                                                                                                               | Authorization                                                                                                                                                                                 |
| ---------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Authentication describes functionality that identifies a user                                                                | Authorization on the other hand is about what a user is allowed to do.                                                                                                                        |
| For example your username and password can authenticate, so identify you, so Google can give you access to your Google Mail. | For example in google drive, authorization checks decide if you are allowed to access a certain document, or if you are only allowed to leave comments or even edit the document.             |
|                                                                                                                              | authorization bugs are about the permissions certain users have, and in big complex applications, especially those used by a big company, you might have very complicated permission systems. |

 a lot of apps have certain default permissions for users, or have permissions that seem harmless, but are actually very powerful. In those cases it might be worth reporting it

roles combine multiple permissions. For example the Owner role or editor role. They are massive roles and give you over 4000 individual permissions. While more fine-grained role like “Cloud Run Admin”, gives 20 permissions

- If the documentation says "Users **must** have the `OWNER` role on the file to change the file name", and you find out that a user with the `EDITOR` role is also able to change the file name, we consider this to be a vulnerability, because the documentation explicitly requires the `EDITOR` role (the word **must** implies an obligation).
- If the documentation says "The `OWNER` of the file **can** change the filename", then we might just fix the documentation to clarify that an `EDITOR` is also able to change the file name (the word **can** implies a possibility, but not an obligation).

- Hunting for authorization bugs doesn’t require a lot of technical knowledge, but it requires a very good understanding of the apps functionality and Summary use-case. This can be done for example by showing a mismatch between the documentation and what is actually implemented.
- explain impact! 
- mismatch between documentation and implementation != mismatch between UI and existing APIs is NOT a bug
### References

[Hunting for Authentication and Authorization Bugs – ft. LiveOverflow (youtube.com)](https://www.youtube.com/watch?v=9xU8j09SScQ)

[Auth Bypass Bugs - Invalid Reports - Learn - Google Bug Hunters](https://bughunters.google.com/learn/invalid-reports/google-products/6121738632691712/auth-bypass-bugs#1-there-can-be-two-or-more-equivalent-permissions)

