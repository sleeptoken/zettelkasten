
2024-09-01 15:12

Source: #android 
### Android prevents you from accidently using unencrypted traffic 

Replacing the button onclick handler with this code
```java
try {  
    URL url = new URL("http://www.android.com/");  
    HttpURLConnection urlConnection = (HttpURLConnection) url.openConnection();  
    InputStream in = new BufferedInputStream(urlConnection.getInputStream());  
    BufferedReader reader = new BufferedReader(new InputStreamReader(in));  
    StringBuilder sb = new StringBuilder();  
    String line;  
    while ((line = reader.readLine()) != null) {  
        sb.append(line).append('\n');  
    }  
    String result = sb.toString();  
    runOnUiThread(() -> hometext.setText(result));  
} catch (Exception e) {  
    e.printStackTrace();  
}
```
that will send an http request to a URL then read the result and show the return text on the screen 
in the logcat output we see an error exception that says cleartext http traffic not permitted

> **If you are using default networking methods provided by android, then it actually tries to prevent you from making the mistake to allow clear text HTTP traffic.**
### INTERNET permissions

Apps need to explicitly ask for internet perms
- change the URL from http to https, app still doesn't work 
- This time we have a security exception: Permission denied (missing INTERNET permission)

below tag written just before the application tag in android manifest
```xml
<uses-permission android:name="android.permission.INTERNET"/>
```
> **In order for apps to be able to access the internet the application has to explicitly request this permission in their android manifest**

unlike application level stuff, networking access through sockets, can be blocked on an operating system level,
if that permission is not there and we try to create a TCP socket, in the logs we can see the security permission exception.
### Cleartext Traffic 

If we want to still allow http cleartext traffic then we need to add an application flag -
```json
android:usesCleartextTraffic="true"
```
this flag allows us to tell the application framework we would like to use cleartext traffic.

> **If an app uses insecure http we probably can see that already in the android manifest** 
#### Alternate method to see perms

using the `network_security_config.xml`, which is a separate XML file in the XML folder, 
apps can be more explicit about these kind of exceptions 
for example - only allowing cleartext traffic for a specific domain 

> search for `android:networkSecurityConfig="@xml/network_security_config"` in the android manifest file 
#### give perms w/o using the application flag 

this cleartext traffic protection is an application level flag i.e. the app can honor it or not
the developer can use TCP sockets directly to send cleartext traffic without having to allow it in the manifest 
we can theoretically bypass this setting by using TCP sockets 

this flag isn't useless because if it is used then we know the app expects to use ClearText traffic. 

in order to intercept these traffics see[[Packet logging with tcpdump]]

---

## Network Interception using burp

most mobile apps use HTTP, we need to configure burpsuite so that we can see manipulate and replay requests

in burp 
1. go to proxy settings
2. change the interface the proxy will listen on from local host to all interface ,i.e. any device on my local network can reach burp including my physical phone 

in emulator
1. go to settings > network & internet > internet > Wi-Fi / network details 
2. then edit the Wi-Fi > change proxy to manual 
3. add the IP of the machine that has burp and the port where burp is listening on 

this allows us to perform a MITM attack against our phone, we are proxying or redirecting HTTP traffic through burp 

- bottom left has an event log that can log errors in [[burpsuite]]
- if we try to set up an HTTP proxy and it doesn't  seems to work, check if we see domains showing up in the event logs
- if nothing shows up ,there's may be a connection problem between phone and our proxy 


### References
[The INTERNET Permission (hextree.io)](https://app.hextree.io/courses/network-interception/android-networking-basics/the-internet-permission)