
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

### References
https://portswigger.net/web-security/learning-paths/race-conditions

turbo intruder - https://blog.intigriti.com/hacking-tools/hacker-tools-turbo-intruder

config and uses Turbo Intruder - https://portswigger.net/research/turbo-intruder-embracing-the-billion-request-attack