
2024-09-01 15:12

Source: #android 

Tags: 
### Lesson 1

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

> If you are using default networking methods provided by android, then it actually tries to 


### References
[The INTERNET Permission (hextree.io)](https://app.hextree.io/courses/network-interception/android-networking-basics/the-internet-permission)