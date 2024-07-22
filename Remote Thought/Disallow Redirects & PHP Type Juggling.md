
2024-07-22 17:21

Source: #natas  #web #py 
### Natas 22 - Disallow Redirects

- on inspecting the source code we find that we have some `php` code at the start that checks if `revelio` is set to 1 
- then it redirects us to the current page 
- in order to stop the redirection we add `allow_redirects =False` in our request
- final code will look as following - 

```
url = 'http://natas22.natas.labs.overthewire.org/?revelio=1'
session = requests.Session()
response = session.get(url,auth=(username,password),allow_redirects=False)
```
### Natas 23 - PHP Type Juggling



### References

N22 - https://www.youtube.com/watch?v=NVuG0Pps-xk
N23 - https://www.youtube.com/watch?v=cB2gBPiL8qE

Type Juggling - https://www.youtube.com/watch?v=-1kftH6t5VA