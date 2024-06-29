
2024-06-19 21:18

Source: #portswigger 

Tags: [[web]]

- Clickjacking is an interface-based attack in which a user is tricked into clicking on actionable content on a hidden website by clicking on some other content in a decoy website.
- This attack **differs from a [[CSRF]]** attack in that the user is required to perform an action such as a button click whereas a CSRF attack depends upon forging an entire request without the user's knowledge or input. 

Protection against CSRF attacks is often provided by the use of a CSRF token: a session-specific, single-use number or nonce. Clickjacking attacks are not mitigated by the CSRF token as a target session is established with content loaded from an authentic website and with all requests happening on-domain. CSRF tokens are placed into requests and passed to the server as part of a normally behaved session. The difference compared to a normal user session is that the process occurs within a hidden `iframe`. 

in clickjaking you got to place an `iframe` in another website and align the buttons in the `iframe` with the buttons on the other website using `css` and reduce transparency of the `iframe` to 0.00001 so that the victim couldn't make out. 

### References
https://portswigger.net/web-security/learning-paths/clickjacking