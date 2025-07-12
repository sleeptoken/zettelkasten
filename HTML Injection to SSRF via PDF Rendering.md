
2025-07-11 22:53

Source: #web 

We have a website which sells online courses. If possible try buying a course & start testing from inside. Try the usual stuff like bypassing payments, accessing locked content, checking for IDORs, and looking for exposed assets.

after completing the course ,the site offered me a certificate generator to download my certificate.

## The Certificate Generator Feature

the page will let you enter:
- Your **name**
- Your **title**
- (Optionally) a quote or message
then you click **“generate”**, and the server gives you back a **PDF or PNG file** with your info on a nice certificate template.

the request body looked something like this:
```json
{  
  "name": "name",  
  "title": "title",  
  "quote": "quote "  
}
```

As always, I started by testing all input parameters for injection. The `title` parameter was the parameter that have no input sanitization.

I replaced it with a simple HTML tag:
```json
"title": "<i>ahmed</i>"
```

Since there was no input sanitization, the HTML payload got rendered in both pdf and image in _italic_ font.

on trying a few more
```json
"title": "<b>Red Team Member</b>"

"title": "<iframe src='https://sub.0xxnum.fun/test'></iframe>"

"title": "<img src=x onerror=alert(1)>"
```

- `<b>` and `<iframe>` **rendered**
- `<script>` and `onerror=` **did not execute**

This meant we had **HTML [[Injection]]**, but **not XSS**.

## Identifying SSRF

next, i wanted to test if this rendering process would actually **fetch resources** from the `iframe`.

so i modified the payload:
```json
"title": "<iframe src='http://sub.tarek.dev/probe'></iframe>"
```
and set up a listener on my end (Burp Collaborator / [[netcat]] / web server).

> I received a DNS and HTTP hit to listener from the IP address of the web application server.

Now i confirmed that **the server was rendering HTML and loading iframe content from our input**.  
that’s **SSRF via HTML Injection** in the PDF rendering flow.

## Going for [[AWS S3]] metadata 

Having SSRF is one thing, but proving its impact is another. Since the server was making outbound HTTP requests, I suspected it might be hosted on **AWS** — and if so, I wanted to reach the **Instance Metadata Service (IMDS)**.




### References
[How I Escalated Simple HTML Injection to SSRF via PDF Rendering | by Ahmed Tarek | Jul, 2025 | Medium](https://medium.com/@0x_xnum/how-i-escalated-simple-html-injection-to-ssrf-via-pdf-rendering-682ea94b3194)