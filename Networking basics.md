
2024-09-01 15:12

Source: #android 

Tags: 
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
### Apps need to explicitly ask for internet perms

- change the URL from http to https, app still doesn't work 
- This time we have a security exception: Permission denied (missing INTERNET permission)

below tag written just before the application tag in android manifest
```xml
<uses-permission android:name="android.permission.INTERNET"/>
```
> **In order for apps to be able to access the internet the application has to explicitly request this permission in their android manifest**

### we can see insecure traffic perms in AndroidManifest

If we want to still allow http cleartext traffic then we need to add an application flag -
```
android:usesCleartextTraffic="true"
```
this flag allows us to tell the application framework we would like to use cleartext traffic.

> **If an app uses insecure http we probably can see that already in the android manifest** 

alternate
using the `network_security_config.xml`, which is a separate XML file in the XML folder, 
apps can be more explicit about these kind of exceptions for example only allowing cleartext traffic for a specific domain 
search for `android:networkSecurityConfig="@xml/network_security_config"` in the android  manifest file 

this cleartext traffic protection is an application level flag i.e. the app can honor it or not
the developer can use TCP sockets directly 
we can theoretically bypass this setting by using TCP sockets 

unlike application level stuff, networking access through sockets, can be blocked on an operating system level 

### References
[The INTERNET Permission (hextree.io)](https://app.hextree.io/courses/network-interception/android-networking-basics/the-internet-permission)