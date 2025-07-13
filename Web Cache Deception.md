
2025-03-14 22:04

Web cache deception exploits cache rules to trick the cache into storing sensitive or private content, which the attacker can then access.

> It's caused by discrepancies between how the cache server and origin server handle requests.

In a web cache deception attack, an attacker persuades a victim to visit a malicious URL, inducing the victim's browser to make an ambiguous request for sensitive content. The cache misinterprets this as a request for a static resource and stores the response. The attacker can then request the same URL to access the cached response, gaining unauthorized access to private information.
## Web caches

A web cache is a system that sits between the origin server and the user. When a client requests a static resource, the request is first directed to the cache. If the cache doesn't contain a copy of the resource (known as a cache miss), the request is forwarded to the origin server, which processes and responds to the request. The response is then sent to the cache before being sent to the user. The cache uses a preconfigured set of rules to determine whether to store the response.

When a request for the same static resource is made in the future, the cache serves the stored copy of the response directly to the user (known as a cache hit).

- Content Delivery Networks (CDNs), which use caching to store copies of content on distributed servers all over the world. CDNs speed up delivery by serving content from the server closest to the user
### Cache keys

- When the cache receives an HTTP request, it must decide whether there is a cached response that it can serve directly, or whether it has to forward the request to the origin server.
- The cache makes this decision by generating a `cache key` from elements of the HTTP request. Typically, this includes the URL path and query parameters, but it can also include a variety of other elements like headers and content type.

If the incoming request's cache key matches that of a previous request, the cache considers them to be equivalent and serves a copy of the cached response.

### Cache rules

Cache rules determine what can be cached and for how long. Cache rules are often set up to store static resources, which generally don't change frequently and are reused across multiple pages.
- Dynamic content is not cached as it's more likely to contain sensitive information, ensuring users get the latest data directly from the server.

> Web cache deception attacks exploit how cache rules are applied, so it's important to know about some different types of rules, particularly those based on defined strings in the URL path of the request. For example:

- Static file extension rules - These rules match the file extension of the requested resource, for example `.css` for stylesheets or `.js` for JavaScript files.
- Static directory rules - These rules match all URL paths that start with a specific prefix. These are often used to target specific directories that contain only static resources, for example `/static` or `/assets`.
- File name rules - These rules match specific file names to target files that are universally required for web operations and change rarely, such as `robots.txt` and `favicon.ico`.
## Constructing a web cache deception attack

1. Identify a target endpoint that returns a dynamic response containing sensitive information. Review responses in Burp, as some sensitive information may not be visible on the rendered page. Focus on endpoints that support the `GET`, `HEAD`, or `OPTIONS` methods as requests that alter the origin server's state are generally not cached.

2. Identify a discrepancy in how the cache and origin server parse the URL path. This could be a discrepancy in how they:
    - Map URLs to resources.
    - Process delimiter characters.
    - Normalize paths.

3. Craft a malicious URL that uses the discrepancy to trick the cache into storing a dynamic response. When the victim accesses the URL, their response is stored in the cache. Using Burp, you can then send a request to the same URL to fetch the cached response containing the victim's data. Avoid doing this directly in the browser as some applications redirect users without a session or invalidate local data, which could hide a vulnerability.
### Using a cache buster

While testing for discrepancies and crafting a web cache deception exploit, make sure that each request you send has a different cache key. Otherwise, you may be served cached responses, which will impact your test results.

As both URL path and any query parameters are typically included in the cache key, you can change the key by adding a query string to the path and changing it each time you send a request.

> [!tip]
> Automate this process using the `Param Miner extension` [[burpsuite]].  To do this, once you've installed the extension, click on the top-level **Param miner > Settings** menu, then select **Add dynamic cachebuster**. Burp now adds a unique query string to every request that you make. You can view the added query strings in the **Logger** tab.

### Detecting cached responses

During testing, it's crucial that you're able to identify cached responses. To do so, look at response headers and response times.

- The `X-Cache` header provides information about whether a response was served from the cache. Typical values include:
    - `X-Cache: hit` - The response was served from the cache.
    - `X-Cache: miss` - The cache did not contain a response for the request's key, so it was fetched from the origin server. In most cases, the response is then cached. To confirm this, send the request again to see whether the value updates to hit.
    - `X-Cache: dynamic` - The origin server dynamically generated the content. Generally this means the response is not suitable for caching.
    - `X-Cache: refresh` - The cached content was outdated and needed to be refreshed or revalidated.
- The `Cache-Control` header may include a directive that indicates caching, like `public` with a `max-age` higher than `0`. Note that this only suggests that the resource is cacheable. It isn't always indicative of caching, as the cache may sometimes override this header.

If you notice a big difference in response time for the same request, this may also indicate that the faster response is served from the cache.

## Exploiting static extension cache rules

Cache rules often target static resources by matching common file extensions like `.css` or `.js`. This is the default behavior in most CDNs.

If there are discrepancies in how the cache and origin server map the URL path to resources or use delimiters, an attacker may be able to craft a request for a dynamic resource with a static extension that is ignored by the origin server but viewed by the cache.
### Path mapping discrepancies

URL path mapping is the process of associating URL paths with resources on a server, such as files, scripts, or command executions. Two common styles are traditional URL mapping and RESTful URL mapping.
#### Traditional URL mapping 

represents a direct path to a resource located on the file system. Here's a typical example:

`http://example.com/path/in/filesystem/resource.html`

#### REST-style URLs

don't directly match the physical file structure. They abstract file paths into logical parts of the API:

`http://example.com/path/resource/param1/param2`

Discrepancies in how the cache and origin server map the URL path to resources can result in web cache deception vulnerabilities. Consider the following example:

`http://example.com/user/123/profile/wcd.css`

- An origin server using REST-style URL mapping may interpret this as a request for the `/user/123/profile` endpoint and returns the profile information for user `123`, ignoring `wcd.css` as a non-significant parameter.
- A cache that uses traditional URL mapping may view this as a request for a file named `wcd.css` located in the `/profile` directory under `/user/123`. It interprets the URL path as `/user/123/profile/wcd.css`. If the cache is configured to store responses for requests where the path ends in `.css`, it would cache and serve the profile information as if it were a CSS file.

### Exploiting path mapping discrepancies

To test how the origin server maps the URL path to resources, add an arbitrary path segment to the URL of your target endpoint. If the response still contains the same sensitive data as the base response, it indicates that the origin server abstracts the URL path and ignores the added segment. For example, this is the case if modifying `/api/orders/123` to `/api/orders/123/foo` still returns order inform2ation.

To test how the cache maps the URL path to resources, you'll need to modify the path to attempt to match a cache rule by adding a static extension. 
- For example, update `/api/orders/123/foo` to `/api/orders/123/foo.js`.
If the response is cached, this indicates:

- That the cache interprets the full URL path with the static extension.
- That there is a cache rule to store responses for requests ending in `.js`.

Caches may have rules based on specific static extensions. Try a range of extensions, including `.css`, `.ico`, and `.exe`.

> [!tip]
> 
> Burp Scanner automatically detects web cache deception vulnerabilities that are caused by path mapping discrepancies during audits. You can also use the [Web Cache Deception Scanner](https://portswigger.net/bappstore/7c1ca94a61474d9e897d307c858d52f0) BApp to detect misconfigured web caches.
#### Lab
##### Identify a path mapping discrepancy

- Go to the Repeater tab. Add an arbitrary segment to the base path, for example change the path to `/my-account/abc`.
- Send the request. Notice that you still receive a response containing your API key. This indicates that the origin server abstracts the URL path to `/my-account`.
- Add a static extension to the URL path, for example `/my-account/abc.js`.
- Send the request. Notice that the response contains the `X-Cache: miss` and `Cache-Control: max-age=30` headers.
	- The `X-Cache: miss` header indicates that this response wasn't served from the cache.
	- The `Cache-Control: max-age=30` header suggests that if the response has been cached, it should be stored for 30 seconds.
- Resend the request within 30 seconds. Notice that the value of the X-Cache header changes to hit. This shows that it was served from the cache. From this, we can infer that the cache interprets the URL path as `/my-account/abc.js` and has a cache rule based on the `.js` static extension. You can use this payload for an exploit.

### Delimiter discrepancies

Delimiters specify boundaries between different elements in URLs. 
- The use of characters and strings as delimiters is generally standardized. For example, `?` is generally used to separate the URL path from the query string. However, as the URI RFC is quite permissive, variations still occur between different frameworks or technologies.

> Discrepancies in how the cache and origin server use characters and strings as delimiters can result in web cache deception vulnerabilities.    Concept similar to [[HTTP Request Smuggling]]

- The [[Java]] Spring framework uses the `;` character to add parameters known as matrix variables. An origin server that uses Java Spring would therefore interpret `;` as a delimiter. It truncates the path after `/profile` and returns profile information.
- Most other frameworks don't use `;` as a delimiter. Therefore, a cache that doesn't use Java Spring is likely to interpret `;` and everything after it as part of the path. If the cache has a rule to store responses for requests ending in `.css`, it might cache and serve the profile information as if it were a CSS file.

Consider these requests to an origin server running the Ruby on Rails framework, which uses `.` as a delimiter to specify the response format:

Encoded characters may also sometimes be used as delimiters. For example, consider the request `/profile%00foo.js`:
- The `OpenLiteSpeed` server uses the encoded null `%00` character as a delimiter. An origin server that uses `OpenLiteSpeed` would therefore interpret the path as `/profile`.
- Most other frameworks respond with an error if `%00` is in the URL. However, if the cache uses `Akamai` or `Fastly`, it would interpret `%00` and everything after it as the path.

#### Exploiting delimiter discrepancies

You may be able to use a delimiter discrepancy to add a static extension to the path that is viewed by the cache, but not the origin server. To do this, you'll need to identify a character that is used as a delimiter by the origin server but not the cache.

Firstly, find characters that are used as delimiters by the origin server. Start this process by adding an arbitrary string to the URL of your target endpoint.

> [!NOTE]
> If the response is identical to the original response, this indicates that the request is being redirected. You'll need to choose a different endpoint to test.

add a possible delimiter character between the original path and the arbitrary string, for example `/settings/users/list;aaa`:
	- If the response is identical to the base response, this indicates that the `;` character is used as a delimiter and the origin server interprets the path as `/settings/users/list`.
	- If it matches the response to the path with the arbitrary string, this indicates that the `;` character isn't used as a delimiter and the origin server interprets the path as `/settings/users/list;aaa` .

Once you've identified delimiters that are used by the origin server, test whether they're also used by the cache. To do this, add a static extension to the end of the path. If the response is cached, this indicates:
	- That the cache doesn't use the delimiter and interprets the full URL path with the static extension.
	- That there is a cache rule to store responses for requests ending in `.js`. 

[list of potential delimiter characters](http://portswigger.net/web-security/web-cache-deception/wcd-lab-delimiter-list )

 For example, consider the payload /`settings/users/list;aaa.js.` The origin server uses `;` as a delimiter:

	- The cache interprets the path as: /settings/users/list;aaa.js
	- The origin server interprets the path as: /settings/users/list
The origin server returns the dynamic profile information, which is stored in the cache.

> [!tip]
> Some delimiter characters may be processed by the victim's browser before it forwards the request to the cache. This means that some delimiters can't be used in an exploit. For example, browsers URL-encode characters like {, }, <, and >, and use # to truncate the path.
> 
> If the cache or origin server decodes these characters, it may be possible to use an encoded version in an exploit.

##### Lab: Exploiting path delimiters 

###### Identify 

In the repeater tab . Add an arbitrary segment to the path i.e. `/my-account/abc`. Send the request. 
	- Notice the `404` Not Found response with no evidence of caching. This indicates that the origin server doesn't abstract the path to `/my-account`.

Remove the arbitrary segment and add an arbitrary string to the original path. For example, change the path to `/my-accountabc`.
	- Send the request. Notice the `404` Not Found response with no evidence that the response was cached. You'll use this response as a reference to help you identify characters that aren't used as delimiters.

brute force possible delimiters in intruder -` /my-account§§abc` 
Under Payload encoding, deselect URL-encode these characters.

the `;` and `?` characters receive a `200` response with your API key. All other characters receive the 404 Not Found response. This indicates that the origin server uses `;` and `?` as path delimiters.
###### Investigate 

Try `/my-account?abc.js` this 
	- Notice that the response doesn't contain evidence of caching. This may indicate that the cache also uses `?` as a path delimiter.
	
Repeat this test using the `;` character instead of `?`. Notice that the response contains the `X-Cache: miss` header.
	- Resend the request. Notice that the value of the `X-Cache header` changes to hit. This indicates that the cache doesn't use `;` as a path delimiter and has a cache rule based on the `.js` static extension. You can use this payload for an exploit.

###### Exploit 

in the exploit server use add the following the body of the request 
```js
<script>document.location="https://YOUR-LAB-ID.web-security-academy.net/my-account;wcd.js"</script>
```
make the victim visit the crafter URL of urs.

### Delimiter decoding discrepancies

Websites sometimes need to send data in the URL that contains characters that have a special meaning within URLs, such as delimiters. To ensure these characters are interpreted as data, they are usually encoded. However, some parsers decode certain characters before processing the URL. If a delimiter character is decoded, it may then be treated as a delimiter, truncating the URL path.

> [!NOTE]
> Differences in which delimiter characters are decoded by the cache and origin server can result in discrepancies in how they interpret the URL path, even if they both use the same characters as delimiters. 

Consider the example `/profile%23wcd.css`, which uses the URL-encoded `#` character:
- The origin server decodes `%23` to `#`. It uses `#` as a delimiter, so it interprets the path as `/profile` and returns profile information.
- The cache also uses the `#` character as a delimiter, but doesn't decode `%23`. It interprets the path as `/profile%23wcd.css`. If there is a cache rule for the `.css` extension it will store the response.

In addition, some cache servers may decode the URL and then forward the request with the decoded characters. Others first apply cache rules based on the encoded URL, then decode the URL and forward it to the next server. These behaviors can also result in discrepancies in the way cache and origin server interpret the URL path

Consider the example `/myaccount%3fwcd.css`:
- The cache server applies the cache rules based on the encoded path `/myaccount%3fwcd.css` and decides to store the response as there is a cache rule for the `.css` extension. It then decodes `%3f` to `?` and forwards the rewritten request to the origin server.
- The origin server receives the request `/myaccount?wcd.css`. It uses the `?` character as a delimiter, so it interprets the path as `/myaccount`.

#### Exploiting delimiter decoding discrepancies

You may be able to exploit a decoding discrepancy by using an encoded delimiter to add a static extension to the path that is viewed by the cache, but not the origin server.

Use the same testing methodology you used to identify and exploit delimiter discrepancies, but use a range of encoded characters. Make sure that you also test encoded non-printable characters, particularly `%00`, `%0A` and `%09`. If these characters are decoded they can also truncate the URL path.


### References
[Web cache deception | Web Security Academy](https://portswigger.net/web-security/web-cache-deception)