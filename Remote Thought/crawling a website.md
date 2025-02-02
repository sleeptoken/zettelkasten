
2025-01-30 18:09

Source: #CTF 

If directory busting and subdirectory discovery doesn't show anything interesting then try this
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

## How crawlers works 

- Crawlers will find keywords in a website by spreading throughout the website like an infection (just for visualization)
- they will then add these keywords in a dictionary, if the crawled website has links to other websites then they will also be crawled.
- when a user searches a keyword, the browser will check if that that keyword exists in the dictionary and if exists then return then show the domain corresponding to the word in the dictionary 

similar words might be used in multiple website, in this case SEO is used to decide the hierarchy for displaying the website 
There is a lot of complexity in how the various search engines individually "point-score" or rank these domains - including vast algorithms.

[SEO Checkup tool](https://pagespeed.web.dev/)

- sitemap of a website is located at `/sitemap.xml`
- robots.txt of a website is located at `/robots.txt`
### References
[TryHackMe | Google Dorking](https://tryhackme.com/r/room/googledorking)