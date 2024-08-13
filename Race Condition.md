
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







### References
https://portswigger.net/web-security/learning-paths/race-conditions