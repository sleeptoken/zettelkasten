
2024-12-21 00:09

Source: #htb #web 

in the challenge description we find a 

I find out that the last dependency on this list — Apache Velocity Engine 1.7 — is affected by [CVE-2020–13936](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-13936). This makes the web application vulnerable to **server side template injection**.

Since we are dealing with an Apache web server. By default, you can view web configurations in the `/etc/apache2/sites-enabled/000-default.conf`


### References
https://medium.com/@rahulhoysala07/hack-the-box-web-challenge-labyrinth-linguist-a67d5005abe0