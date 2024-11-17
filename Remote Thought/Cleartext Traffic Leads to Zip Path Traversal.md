
2024-11-06 12:38

Source: #android 

We have an app that basically downloads maps and lets you view it
The goal will be to use network interception to exploit an issue that allows us to write arbitrary files.
- to write a `hax` file into the pocket maps download folder

There are lots of different angles how you could approach this application
- you could go with static analysis, [[Reverse Engineering Android apps]] approach using JADX to decompile the APK and look around. 
- you could set up [[Advanced Interception Tricks]] and look at all the different kinds of HTTP requests the app sends 
### Static 

Using JADX, we can explore the Android manifest, but we can also see a network security config. 
 >This config allows clear text traffic for a particular domain and this IP, but is this domain actually used even If clear
> text is permitted in the config? Maybe these are just for testing and they are not actually used in production.

We can search for this domain and we find this API request. So it does seem relevant, but we haven't really confirmed it yet. That's why we could now jump over to the dynamic testing approach using network interception.
### Dynamic 

we can see the application does indeed use HTTP Clear Text Communication. 
This allows a machine in the middle attacker to not only see the traffic that is sent and received, but also to manipulate the traffic. 

When we download a map, we see the request for it in Burp, which turns out to be a `ghz` file. which actually is just a zip archive. 
this zip archive contains a lot of files that you can see when you unpack it locally. 
(right click on the request and `copy URL` and paste it in the browser, replace the domain name to `storage.googleapis.com`)

Now let's try to find this content on the phone using ADB shell and make sure you have root access we can use
``` sh
find / | grep pocket 
# grab for any file parts containing pocket map
```
we can find the downloaded map here **This shows us that the app does indeed unpack the map zip**
### MITM setup

We are going to create a dynamic test setup to simulate a machine in the middle attacker in order to exploit the pocket text map.

head over to Burp and see the two important requests.
- One loads the available maps 
- the other is the GHZ zip archive download. 

And keep in mind, our goal is to attack this app by simulating a machine in the middle [[MITM]] attacker who can manipulate the clear text request or responses. Of course, we could simply intercept the traffic with [[burpsuite]] or even use match and replace rules, but all of that fails once we try to handle binary data such as the zip file.
#### HTTP mock extension 

First, I'm going to Install a burp extension called HTTP Mock. This extension gives us a lot more freedom in the way how we can change and manipulate the responses to certain requests.

To do that, we can right click our URLs and select extension HTTP mock - -> mock HTTP response and send these details over to the extension. 

Here we have the mock rules using regular expressions. Different parts of the requests are matched. Basically it works this way. If a request is coming into Burp, it will check the host, the port, the method, and even the file path. And if it matches, then respond with this data instead. 

But we are going one step further. Now we are actually going to use the redirect to URL feature (bottom of the page) set to Basic by default, to send the request to another web server. 

replace the contents of the response to `http://127.0.0.1:1234/map.json`
we are actually sending a request to this Local server, URL, I'm also removing part of the URL matching in order to match any file that we download (append `/.*`)
change some other fields (eg. change host & Port to `.*`) so they match anything. This way we can make sure our rules really get applied to all the incoming requests.
### Static analysis 

we could search for map download related code or we go straight to zip functionality searching for typical Android Java classes related to zip files such as ZipEntry.

> The entry name of a zip file can contain a path traversal. for this current app

This means we just have to create a zip file with an entry that contains such a path traversal (the app creates a directory if it doesn't exists . 

create a python file names `server.py`

```python
from flask import Flask, jsonify, send_file
import zipfile, io

app = Flask(__name__)

@app.route('/map.json')
def serve_map():
    # This is where you would define the JSON response or fetch it from a file or database
# to confirm here that our custom request redirection worked. We are only seeing one app exactly as we have specified in our fake server.
    response = {
        "maps-0.13.0_0-path": "maps",
        "maps-0.13.0_0":
        [
            { 
                "name": "hextree-android_continent", 
                "size": "812K", "time": "2024-02" },
        ]
    }
    return jsonify(response)

@app.route('/map.zip')
def map_archive():
# create a malicious zip file that leads to an arbitrary file. Specifically try to write the hex file into the pocket maps download folder.
    buffer = io.BytesIO()
    with zipfile.ZipFile(buffer, 'w') as zipf: # create a zip file in a memory buffer
        zipf.writestr('../../downloads/hax','test path traversal') # using the zipfile module we can create arbitrary zip file entries with any name and content
    buffer.seek(0)
    return send_file(buffer, as_attachment=True,download_name='map.ghz')

if __name__ == '__main__':
    app.run(debug=True, port=1234)
```

we can now simulate our machine in the middle attack where we as the attacker manipulate the map download. When a user tries to download any map, the unzipping code will run and will write our malicious hacks file into an arbitrary folder that it was not intended to write into. 
### Report 

We have Identified two issues which lead to a quite serious security issue in the pocket text map.
- The first issue makes the attack possible in the first place. It turns out that the app uses insecure clear text HTTP in order to download map data. This insecure setup allows an attacker to perform a machine in the middle attack and could manipulate the downloaded map data.
- The second issue was found in how the map unzip code works. an attacker can craft a malicious map zip file, including a path traversal that can be used to write arbitrary files and arbitrary file locations.

It's recommended to sanitize the zip entry name, so they cannot include path traversals and the API should use SSL instead.

### References
[Static vs. Dynamic Analysis](https://app.hextree.io/courses/network-interception/case-study-pockethexmap)
