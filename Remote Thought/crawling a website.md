
2025-01-30 18:09

Source: #CTF 

if directory busting and subdirectory discovery doesn't show anything interesting then try this
### Gospider

```sh
gospider -s http://vulnnet.thm -c 10 -d 0 -w -t 10 — robots -a -r -v
```
### LinkFinder

```sh
git clone https://github.com/GerbenJavado/LinkFinder.git
pip3 install -r requirements.txt  
python3 linkfinder.py -d -i http://vulnnet.thm/ -o cli
```
Now we use [LinkFinder](https://github.com/GerbenJavado/LinkFinder) to find links in JavaScript files

### References
