
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
#### *Lab*

This lab's login mechanism uses rate limiting to defend against brute-force attacks. However, this can be bypassed due to a race condition. 

> 1. Observe that if you enter the incorrect password more than three times, you're temporarily blocked from making any more login attempts for the same account.
> 2. Try logging in using another arbitrary username and observe that you see the normal Invalid username or password message. This indicates that the rate limit is enforced per-username rather than per-session.
> 3. Deduce that the number of failed attempts per username must be stored server-side.
> 4. Consider that there may be a race window between:
	 - When you submit the login attempt.
     - When the website increments the counter for the number of failed login attempts associated with a particular username. 

 Gist - find a `POST /login` request containing an unsuccessful login attempt for your own account. create a group with 20 similar requests and send it in parallelly. Notice that all the all the responses have a invalid pass message
 
Send the group of requests in sequence, using separate connections to reduce the chance of interference. For details on how to do this, see Sending requests in sequence.

Observe that after two more failed login attempts, you're temporarily locked out as expected.


for single packet attack we use the `race-single-packet-attack.py` template 
change the username to `admin` and password to `%s`
```python
for password in wordlists.clipboard:
    engine.queue(target.req, password, gate='1')
```

### References
https://portswigger.net/web-security/learning-paths/race-conditions