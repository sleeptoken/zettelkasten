
2025-03-05 23:51

#### A note about safe-fetch

The challenge uses a wrapper over `fetch` called `safe_fetch`, which enforces every URL passed be

- on a valid domain
- not in a private IP range
- provided over HTTPS

The primary purpose of that is to avoid any potential, unintended SSRF, as well as better simulate a more realistic environment.

#### So, about that OIDC

OpenID Connect is a very complex and convoluted protocol. For the sake of this challenge, the implementation used by the bad-todo app is the simplest possible implementation of OIDC possible. Because of that, only rudimentary verification of input data is performed.
- As such, only the following 4 fields in the `/.well-known/openid-configuration` are necessary:
	- `issuer`
	- `authorization_endpoint`
	- `token_endpoint`
	- `userinfo_endpoint`

If we provide a web server hosting this configuration as the authorization server, a session cookie is set and we are then redirected to the `authorization_endpoint` with the following parameters:

- `client_id` - The user-supplied OIDC client ID
- `redirect_uri` - The base URL of the challenge with `/auth_redirect`
- `scope=openid` - Requests only basic permissions
- `response_type=code`
- `state=<random string>` - In real implementations, this should be a per-auth-session string, but for simplicity’s sake I just used the value of the session cookie. The purpose of this parameter is to avoid replay attacks and CSRF attacks.

In a real identity provider, this endpoint would present the user with an authentication prompt, request consent and do internal sanity checks. Once that’s done, the identity provider redirects the user to `/auth_redirect` with 2 relevant fields in the query:

- `code` - The authentication code used to obtain a login token
- `state` - That same state string as above

The code is then used to obtain a token against `token_endpoint`. Because this is a “public client”, we don’t need any extra authentication against the token endpoint. We just send it

- `code` - The code we got from the user
- `grant_type=authorization_code`
- `redirect_uri` - The same redirect URI as we used earlier
- `client_id` - The client ID we passed at the start

This gives us a response with an `access_token` and `token_type` which we then use to authenticate to the userinfo endpoint. The response structure of the userinfo is basically undocumented, with each IDP having slightly different response fields, but one field remains the same and proper across all of them: `sub`. The `sub` is the IDP-specific user ID and can be an arbitrary string. Once we receive the userinfo, we set a few cookie values to identify the user as logged in, and redirect them back to the homepage of the app.

#### The vulnerability

This to-do list app makes use of a “database-per-user” architecture, an approach popular with apps that are built on SQLite. In that approach, every user has their own on-disk database. This is implemented in `storage.js` in the challenge handout.

The base vulnerability is insufficient validation and sanitization of the `sub` for naming a file on disk. This vulnerability has 2 parts to it:
[[saniti]]
- `sanitizePath` only checks if the path is within the `STORAGE_LOCATION`, not within a specific directory for a given identity provider. This allows for accessing data stored by other users.
- `getStoragePath` relies on `encodeURIComponent` to remove any slashes from the user-controlled input, but _also_ splits it (for storage optimization reasons - you don’t want all files in one directory). This allows you to set the first 2 characters of your `sub` to `..` and get a relative path.

Therefore, by setting our sub to `..flag`, we can access the “backed up” database of the admin user. Doing so allows us to log in and read the admin’s todo

#### Final notes and further reading

Due to the nature of this app, the admin’s database has to be set as `readonly`, otherwise the first team to log in could have vandalized it. Therefore, if you try to take any action as the admin, you will be met with SQLite errors.

External links:

- [Writeup by ireland.re - Their approach makes use of just one json payload to handle all 3 endpoints, a clever take on this solution](https://ireland.re/posts/irisctf_2025/#webbad-todo-75-solves)
- [Source code of my solution](https://gist.github.com/rphsoftware/bc7a98428fe538131a584e33cfc6e243)

### References
https://rph.space/blog/irisctf-2025-bad-todo/