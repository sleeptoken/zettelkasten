
2024-06-27 20:15

Source: #portswigger 

Tags: [[web]]
## Recon
#### Tools: 

- You can use **Burp Scanner** to crawl the application, then manually investigate interesting attack surface.
- Burp Scanner automatically extracts some endpoints during crawls, but for a more heavyweight extraction, use the **JS Link Finder BApp**.
- You can use the built-in **HTTP verbs list** in Burp Intruder to automatically cycle through a range of HTTP methods. 
- You can use the **Content type converter BApp** to automatically convert data submitted within requests between XML and JSON.
#### API Documentation 

Even if API documentation isn't openly available, you may still be able to access it by browsing applications that use the API. To do this, you can use Burp Scanner to crawl the Application.

While browsing the application, look for patterns that suggest API endpoints in the URL structure, such as `/api/`. Also look out for JavaScript files. These can contain references to API endpoints that you haven't triggered directly via the web browser. 
##### Interacting with API endpoints

- observe the API's behavior and discover additional attack surface eg. you could investigate how the API responds to changing the HTTP method and media type. 
- **review error messages and other responses closely.** Sometimes these include information that you can use to construct a valid HTTP request. 
- **Identifying supported HTTP methods** - GET,POST,OPTIONS etc.
- **Identifying supported content types** -
	- To change the content type, modify the `Content-Type` header, then reformat the request body accordingly. 
	- Changing the content type may enable you to:
	    - Trigger errors that disclose useful information.
	    - Bypass flawed defenses.
	    - Take advantage of differences in processing logic. For example, an API may be secure when handling JSON data but susceptible to injection attacks when dealing with XML.
	



### References

test cases - 
https://www.evernote.com/shard/s433/client/snv?isnewsnv=true&noteGuid=588ed6bf-f893-4953-aeb2-d4f2260fd72f&noteKey=6fe301e3f61af998&sn=https%3A%2F%2Fwww.evernote.com%2Fshard%2Fs433%2Fsh%2F588ed6bf-f893-4953-aeb2-d4f2260fd72f%2F6fe301e3f61af998&title=API%2BTest%2BCases%2BCreation%2B%2528API%2BTesting%2B-%2BPart%2B33%2529