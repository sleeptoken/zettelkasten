
2024-07-09 14:02

Source: #web 

you can get false positives in things like burp suite where you're receiving potential `ssrf` scenarios but it's only a [[DNS (53 port)]] callback 

 burp suite told that there was a [[Server-side request forgery (SSRF)]] potential because burp collaborator had a `dns` response that in itself lacks the impact to be a business addressable impact ( the kind of finding that would typically have a payment behind it) it is the indication that you might want to go further but you are still at the point where you don't quite have a finding you just have a lead to a finding

**what's the risk?** 
can i use this request and make `http` requests with it if you can, can you then use that to try and identify an internal server that you can request against if that's not the case if you could only request other servers on the internet is there a rate limit kind of element here or could this turn this server into something that is going to cause resource exhaustion for itself or for other servers on the internet and at that point you'd stop
#### False Positives and Expected Functionality

people emailing using an email-based form seeing about collaborator hit and getting excited and thinking there's a finding but it's actually [[SMTP (25 port)]] working as intended it's just emailing your collaborator address
that doesn't mean you have any impact sitting behind it, but it does mean that the input was consumed which can maybe tell you something about the inner workings of the application but isn't telling you that there's a security risk their 
1. Or is this just a [[webhook]] processing what i'm providing it and its work is intended
2. maybe a web application firewall ([[WAF]]) is picking up from the logs and processing but you don't actually have a hook into any sort of internal system or anything outside, 
3. maybe you've got an internal `ip` address  disclosure but that in itself isn't going to be payable and isn't a significant event you need  to go deeper than that
### References
[Server-side Request Forgery Misconceptions – ft. Codingo (youtube.com)](https://www.youtube.com/watch?v=MNbmsY0j7r8)