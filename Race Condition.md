
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





### References
https://portswigger.net/web-security/learning-paths/race-conditions