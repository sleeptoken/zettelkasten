
2024-08-08 17:20

Source: #portswigger #web 
### Limit overrun race conditions

The most well-known type of race condition enables you to exceed some kind of limit imposed by the business logic of the application. 

example - a website that accepts a coupon code from a user performs the following steps - 
    1. Check that you haven't already used this code.
    2. Apply the discount to the order total.
    3. Update the record in the database to reflect the fact that you've now used this code.
If you later attempt to reuse this code, the initial checks performed at the start of the process should prevent you from doing this
Now consider what would happen if a user who has never applied this discount code before tried to apply it twice at almost exactly the same time, the application transitions through a temporary sub-state i.e. a state that it enters and then exits again before request processing is complete.

 There are many variations of this kind of attack, including:
>     1. Redeeming a gift card multiple times
>     2. Rating a product multiple times
>     3. Withdrawing or transferring cash in excess of your account balance
>     4. Reusing a single CAPTCHA solution
>     5. Bypassing an anti-brute-force rate limit

**Limit overruns are a subtype of so-called "time-of-check to time-of-use" (TOCTOU) flaws.** 
### Detecting and exploiting limit overrun race conditions 

in high level terms all you need to do is - 
- Identify a single-use or rate-limited endpoint that has some kind of security impact or other useful purpose.
 - Issue multiple requests to this endpoint in quick succession to see if you can overrun this limit.
The primary challenge is timing the requests so that at least two race windows line up, causing a collision. This window is often just milliseconds and can be even shorter. 

The single-packet attack enables you to completely neutralize interference from network jitter by using a single TCP packet to complete 20-30 requests simultaneously. 
Although you can often use just two requests to trigger an exploit, sending a large number of requests like this helps to mitigate internal latency, also known as server-side jitter. 
#### *Lab*

> without the session cookie, you can only access an empty cart. From this, you can infer that:
- The state of the cart is stored server-side in your session.
- Any operations on the cart are keyed on your session ID or the associated user ID.
This indicates that there is potential for a collision.
##### Benchmark the behavior

1. Make sure there is no discount code currently applied to your cart.
2. Send the request for applying the discount code (`POST /cart/coupon`) to Repeater.
3. In Repeater, create multiple instances of the request and add the new tabs to a group. 
4. Select send group in parallel (single packet attack), effectively applying the discount code multiple times at once.
### Detecting and exploiting limit overrun race conditions with Turbo Intruder

Turbo Intruder is suited to more complex attacks, such as ones that require multiple retries, staggered request timing, or an extremely large number of requests.

 To use the single-packet attack in Turbo Intruder:
 
- Ensure that the target supports `HTTP/2`. The single-packet attack is incompatible with `HTTP/1`.
- Set the `engine=Engine.BURP2` and `concurrentConnections=1` configuration options for the request engine.
- When queueing your requests, group them by assigning them to a named gate using the gate argument for the `engine.queue()` method.
- To send all of the requests in a given group, open the respective gate with the `engine.openGate()` method.
#### *Lab* - Turbo Intruder [[burpsuite]]

This lab's login mechanism uses rate limiting to defend against brute-force attacks. However, this can be bypassed due to a race condition. 
###### Predict a potential collision

> 1. Observe that if you enter the incorrect password more than three times, you're temporarily blocked from making any more login attempts for the same account.
> 2. Try logging in using another arbitrary username and observe that you see the normal Invalid username or password message. This indicates that the rate limit is enforced per-username rather than per-session.
> 3. Deduce that the number of failed attempts per username must be stored server-side.
> 4. Consider that there may be a race window between:
	 - When you submit the login attempt.
     - When the website increments the counter for the number of failed login attempts associated with a particular username. 
###### Benchmark the behavior

1. Send the group of requests in sequence, using separate connections to reduce the chance of interference. Observe that after two more failed login attempts, you're temporarily locked out as expected.

2. Send the group of requests again, but this time in parallel. Study the responses. Notice that although you have triggered the account lock, more than three requests received the normal Invalid username and password response.
###### Prove the concept

1. Right-click on the `POST /login` and select Extensions > Turbo Intruder > Send to turbo intruder.
2. In Turbo Intruder, in the request editor, mark the `password` parameter as a payload position with the `%s` placeholder.
3. Change the `username` parameter to `carlos`.
4. From the drop-down menu, select the `examples/race-single-packet-attack.py` template.
5. copy the password list and replace the for loop with the one below
```python
for password in wordlists.clipboard:
    engine.queue(target.req, password, gate='1')
```
###### Alternate POC

```python
for word in open('/usr/share/wordlists/rockyou.txt').readlines()[0:20]:
	# check for first 20 passwords 
	engine......
def handleResponse(req,interesting):
	if req.status_coded == 302:      or    	if "successful" in req.response:
		table.add(req)
	# add them to the list only if they get 302
```

Essentially in the script we're queueing 20 requests into this gate and then at the end we're going to open a gate so they all get sent at once
### Hidden multi-step sequences

In practice, a single request may initiate an entire multi-step sequence behind the scenes, transitioning the application through multiple hidden states that it enters and then exits again before request processing is complete. We'll refer to these as "sub-states".

If you can identify one or more HTTP requests that cause an interaction with the same data, you can potentially abuse these sub-states to expose time-sensitive variations of the kinds of logic flaws that are common in multi-step workflows. 
#### Methodology 
refer - https://portswigger.net/research/smashing-the-state-machine
##### 1 - Predict potential collisions 
ask the following questions :
- Is this endpoint security critical? Many endpoints don't touch critical functionality, 
- Is there any collision potential? For a successful collision, you typically need two or more requests that trigger operations on the same record.
##### 2 - Probe for clues

To recognize clues, you first need to benchmark how the endpoint behaves under normal conditions. You can do this in Burp Repeater by grouping all of your requests and using the `Send group in sequence (separate connections)` option
Next, send the same group of requests at once using the single-packet attack (or last-byte sync if HTTP/2 isn't supported) to minimize network jitter. just like we did in previous 2 labs
##### 3 - Prove the concept

It may help to think of each race condition as a structural weakness rather than an isolated vulnerability. 
### Multi-endpoint race conditions

Perhaps the most intuitive form of these race conditions are those that involve sending requests to multiple endpoints at the same time. 
#### Aligning multi-endpoint race windows

When testing for multi-endpoint race conditions, you may encounter issues trying to line up the race windows for each request, even if you send them all at exactly the same time using the single-packet technique.
This common problem is primarily caused by the following two factors:

- **Delays introduced by network architecture** - For example, there may be a delay whenever the front-end server establishes a new connection to the back-end. The protocol used can also have a major impact.
 - **Delays introduced by endpoint-specific processing** - Different endpoints inherently vary in their processing times, sometimes significantly so, depending on what operations they trigger.
#### Connection warming

Back-end connection delays don't usually interfere with race condition attacks because they typically delay parallel requests equally, so the requests stay in sync.

It's essential to be able to distinguish these delays from those caused by endpoint-specific factors. One way to do this is by "warming" the connection with one or more inconsequential requests to see if this smoothens out the remaining processing times.

- If the first request still has a longer processing time, but the rest of the requests are now processed within a short window, you can ignore the apparent delay and continue testing as normal.

- If you still see inconsistent response times on a single endpoint, even when using the single-packet technique, this is an indication that the back-end delay is interfering with your attack. You may be able to work around this by using Turbo Intruder to send some connection warming requests before following up with your main attack requests.
##### *Lab* - connection warming

###### Predict - identify potential collision 

have something in the cart and on removing the session cookies from the request we see our cart is empty this is telling us that the state of the cart is stored server side in our session and any operations on the cart will be **keyed onto the session ID** and that does indicate a potential Collision
we should also notice that whenever we made the successful purchase it was also part of **a single request response cycle** and that creates the opportunity for race condition
###### Probe - benchmark the behavior  

- get the POST request in repeater that is made to add items in the cart, that shows the productid & quantity and also add the checkout request, then create a group 
- send the single connection request, we see that the 1st req. came back in 441 milliseconds the second one came back in 137
- This is known as warming the connection so to do that we could potentially add a request to our repeater group of the homepage 

> essentially we added that inconsequential request at the beginning just making a call to the home home page and that warmed the connection meaning that the time between the second and third requests were a lot smaller, it created a smaller window and this is an indication that the delay is caused by the backend Network architecture rather than the respective processing time of each endpoint and therefore it's not likely to interfere with the attack
###### Prove - Exploit the race condition 

when we change the productid in the 2nd request from the group to an expensive product and send the group as single connection then we see the last checkout response says insufficient funds which is exactly what we expected 

- add the cheaper item in the cart and remove everything else
- and having the same group of requests in repeater we change the product id to the expensive item's product id 
- and send the whole groups request in parallel

what happened there we had the cheaper item in our carts and we purchased the cheaper item using the `POST cart/checkout` but at the same time we sent another request , it didn't change the item because we still had the cheaper item there but it added the expensive leather jacket to the cart and we basically exploited that race window so in between the time of it checking the cart and then actually making the purchase we were able to update the cart's contents
##### Alternate 

in the last step try changing the quantity of the cheaper item if the cheaper item is a gift card, then buy sufficient gift cards (its usually limited to 3 digits to 2 digits so change quantity to `99`) and buy the expensive item.
#### Abusing rate or resource limiting

***if connection warming doesn't make any difference*** there are various solutions to the problem using turbo Intruder you can introduce a short client side delay however as this involves splitting your actual attack results across multiple `TCP` packets you won't be able to use a single packet attack technique as a result on **high jittered** targets the attack is unlikely to work reliably regardless of what delay you set.

By sending a large number of dummy requests to intentionally trigger the rate or resource limit, you may be able to cause a suitable server-side delay. This makes the single-packet attack viable even when delayed execution is required. 
### Single-endpoint race conditions

 For this attack to work, the different operations performed by each process must occur in just the right order. It would likely require multiple attempts, or a bit of luck, to achieve the desired outcome.

Email address confirmations, or any email-based operations, are generally a good target for single-endpoint race conditions. Emails are often sent in a background thread after the server issues the HTTP response to the client, making race conditions more likely. 
##### *Lab*

you send an email change request to `admin@email` and an email change request link is sent to `admin@email `
we don't have access to `admin@email`
we have access to our exploit mail server that receives emails from any user using the exploit server subdomain.
###### Predict a potential collision

- Try submitting two different email addresses in succession, then go to the email client w same subdomain.
- Notice that if you try to use the first confirmation link you received, this is no longer valid. From this, you can infer that the website only stores one pending email address at a time. As submitting a new email address edits this entry in the database rather than appending to it, there is potential for a collision.
###### Benchmark the behavior\

- create a group of `POST /my-account/change-email` requests in Repeater, duplicate it with a different email id w same subdomain 
- Send the group of requests in sequence over separate connections. 
- we see that in the email server we have a confirmation link for all the emails we added, as expected
###### Probe for clues

1. in Repeater, send the group of requests again, but this time in parallel, effectively attempting to change the pending email address to multiple different values at the same time. 
2. Go to the email client and study the new set of confirmation emails you've received. Notice that, this time, the recipient address doesn't always match the pending new email address.
3. Consider that there may be a race window between when the website:
    - Kicks off a task that eventually sends an email to the provided address.
    - Retrieves data from the database and uses this to render the email template.
4. Deduce that when a parallel request changes the pending email address stored in the database during this window, this results in confirmation emails being sent to the wrong address.
###### Prove the concept

- create a new group of only 2 requests of `POST /my-account/change-email`
- one having the email of the admin and the other having the email w our email server domain 
- send the group in parallel, we get an email asking us to confirm to change the email to `admin@email`
### Session-based locking mechanisms

Some frameworks attempt to prevent accidental data corruption by using some form of request locking. For example, PHP's native session handler module only processes one request per session at a time.
If you notice that all of your requests are being processed sequentially, try sending each of them using a different session token.
### Partial construction race conditions

Many applications create objects in multiple steps, which may introduce a temporary middle state in which the object is exploitable.

For example, when registering a new user, an application may create the user in the database and set their API key using two separate SQL statements. This leaves a tiny window in which the user exists, but their API key is uninitialized.

This kind of behavior paves the way for exploits whereby you inject an input value that returns something matching the uninitialized database value, such as an empty string, or null in JSON, and this is compared as part of a security control. 

It's possible to cause similar partial construction collisions with a password rather than an API key. However, as passwords are hashed, this means you need to inject a value that makes the hash digest match the uninitialized value. 
### Time-sensitive attacks

When high-resolution timestamps are used instead of cryptographically secure random strings to generate security tokens.

Consider a password reset token that is only randomized using a timestamp. In this case, it might be possible to trigger two password resets for two different users, which both use the same token. All you need to do is time the requests so that they generate the same timestamp.
##### *Lab*

we have a similar setup as the previous lab, a known email, pass and its email server and a privileged user whose password we don't know
###### Explore password reset functionality

let's go and try and reset our password straight away, we send the request for the known user and we get an email with the link to reset the password and we'll see that we do have this token it's quite a long token doesn't look like something that we'd easily be able to guess or predict 

taking the POST /for request in repeater we see that it has a phpsessionid, a csrf token and a username .
###### Bypass the per-session locking restriction

###### Exploit timing issue to reset password

### How to prevent race condition vulnerabilities
### References 
https://portswigger.net/web-security/race-conditions

turbo intruder - https://blog.intigriti.com/hacking-tools/hacker-tools-turbo-intruder

config and uses Turbo Intruder - https://portswigger.net/research/turbo-intruder-embracing-the-billion-request-attack