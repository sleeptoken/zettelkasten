
2024-06-27 10:13

Source: #youtube 

Tags: [[XSS]] [[web]]

advantages of using alert - 
- alert is a function, that is part of the ` window object`. And the `window object` holds the most critical data an attacker might be  interested in. For example the `localStorage`, which might contain session tokens. Or the  famous `window.document.cookies`

`alert(document.domain)` - tells on which domain `xss` is actually executed on

when you use` alert(document.domain)` and see that the domain on alert is different from the domain in the `url` bar, the site is using an `iframe` and the `xss` is located on that domain 

Google uses a range of sandbox domains to safely host user-generated content. Many of these sandboxes are specifically meant to isolate user-uploaded HTML and JavaScript and make sure that they can't access any user data. - known as `sandboxed subdomains`

`iframe` can also have a sandbox attribute - known as `sandboxed iframes` - is isolated from the website it is embedded into 

sometimes alert is blocked due to a sandboxed `iframe` in that case use `console.log("XSSTEST:"+window.origin)`

![[Pasted image 20240627104732.png]]
When you have for example  an injection into a JSONP sandboxed iframe, the actual site usually  communicates with the iframe via postMessages. And so there could be a way to exploit a message to get XSS on the actual site. Basically doing a sandbox escape. But in this case the vulnerability is **NOT** this first XSS, the vulnerability is the **ESCAPE** out of it. So reporting the sandboxed XSS itself **DOES  NOT** qualify for a bounty. But it means, if you can somehow **CHAIN** other bugs, escalate it, and execute JavaScript on the **ACTUAL IN-SCOPE** domain, then it would be a valid issue you can report.

### References 

https://www.youtube.com/watch?v=KHwVjzWei1c&list=PLhixgUqwRTjx2BmNF5-GddyqZcizwLLGP&index=28