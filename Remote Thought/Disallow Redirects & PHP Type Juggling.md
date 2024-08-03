
2024-07-22 17:21

Source: #natas  #web #py 
### Natas 22 - Disallow Redirects

- on inspecting the source code we find that we have some `php` code at the start that checks if `revelio` is set to 1 
- then it redirects us to the current page 
- in order to stop the redirection we add `allow_redirects =False` in our request
- final code will look as following - 

```python
url = 'http://natas22.natas.labs.overthewire.org/?revelio=1'
session = requests.Session()
response = session.get(url,auth=(username,password),allow_redirects=False)
```

### Natas 23 - PHP Type Juggling

snippet of source code below
```python
if(strstr($_REQUEST["passwd"],"iloveyou") && ($_REQUEST["passwd"] > 10 )){
```
`strstr()` function - find the first occurrence of a string
PHP doesn't entirely care what your variable really is, it all depends on what context you're using it in there 
because of the 2nd condition in the if statement we will add a number >10 at the start of our password 
thus we will add `11iloveyou`  to retrieve the password

### Natas 24 - PHP Type Juggling (strcmp)

in our webiste source we have the following code that is vulnerable to type juggling
```python
if(!strcmp($_REQUEST["passwd"],"<censored>")){
```
strcmp Returns: 
	- `-1` if `string1` is less than `string2`
	- `1` if `string1` is greater than `string2`, 
	- `0` if they are equal.
	
we change the REQUEST variable to an array instead of an expected string, because we're passing this in as an HTTP POST variable and we're passing it in raw we're able to modify it and let's say the password variable is no longer a string we can denote it to be an array by adding  square braces kind of at the very end of the variable name. our POST request will look like - 
```python
response = session.post(url,data={'passwd[]':'any'},auth=(username,password))
```
output throws a warning and gives the pass.

### References

N22 - https://www.youtube.com/watch?v=NVuG0Pps-xk
N23 - https://www.youtube.com/watch?v=cB2gBPiL8qE

Type Juggling - https://www.youtube.com/watch?v=-1kftH6t5VA
strcmp - https://www.php.net/manual/en/function.strcmp.php