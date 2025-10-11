
2025-10-11 10:38

Source: #web

## Session fixation 

Session fixation is a type of attack where the attacker sets the session identifier before the victim logs in. When the user authenticates with the same session ID, the attacker can reuse it to access the session without needing credentials. This exploit takes advantage of poor session management practices, such as not regenerating session IDs after login. 

Visually explained [here](https://owasp.org/www-community/attacks/Session_fixation)

## Session hijacking

Session hijacking is a type of attack where hackers take control of an active user session by stealing and exploiting the session ID. The session ID is a unique token that identifies the user and maintains state across requests, often stored in cookies, passed in URLs, or embedded in hidden form fields. In session hijacking attacks, once the attacker obtains the session ID, they can access the user’s account without needing credentials, allowing them to read sensitive data, make unauthorized changes, or escalate privileges. Timing is critical in these attacks, as session IDs are only valid for a limited period.

### Versus 

The session fixation attack is not a class of Session Hijacking, which steals the established session between the client and the Web Server after the user logs in. Instead, the Session Fixation attack fixes an established session on the victim’s browser, so the attack starts before the user logs in.

### Other attacks

- Session predictions. The attacker guesses or predicts valid session identifiers based on weak generation algorithms. This can be surprisingly effective if session tokens follow a pattern or are not randomized properly.

- Session replay. In this attack, the attacker captures a valid session request and replays it later to impersonate a user. It often overlaps with hijacking, especially in API-based applications.

- Session spoofing. Here, an attacker manually crafts session data or headers to impersonate a session, typically when session validation is weak or token structure is predictable.

### Reference 

https://nordstellar.com/blog/session-fixation-vs-session-hijacking/
