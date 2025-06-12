
2025-06-12 12:38

Source: #android  
# Intent Redirect 

Let's talk about the vulnerability, or rather, the vulnerable code pattern that led to many issues in many apps. It's often called Intent Forwarding 
As you know, intents are complex objects that can carry a lot of different values, including other intents. 

this is the code that is reason for this vulnerability found in flag5 in the attack surface app 
```java
		if (this.nextIntent.getStringExtra("reason").equals("back")) {
		    this.f.addTag(this.nextIntent.getStringExtra("reason"));
		    success(this);
		} else if (this.nextIntent.getStringExtra("reason").equals("next")) { //a different condition that led to a `startActivity` call
		    intent.replaceExtras(new Bundle());
		    startActivity(this.nextIntent);
}
```
(above code is an extension of the code written in flag5 attack surface app)

- And if you look carefully, it takes one of the intents inside the original intent And this is what we call an Intent Redirect or Intent Forwarding.  

The attacker created an original intent which contained another intent, and the target app extracts this extra intent and calls `startActivity` with it and this is bad. 

This can be abused in two main ways. 

1. It allows you to attack non-exported activities.
	`android:exported="false"`

> [!important]
> 	Activities can be started internally by the app itself, which we have seen earlier when we played around with intents. And now look at this code again. Here we have a `startActivity` within the app where the attacker controls the intent details. 

So the idea should be clear. If you manage to craft an intent targeting the non-exported activity and place it into the intent that leads to this vulnerable `startActivity` call, your intent will be redirected or forwarded to the non-exported activity. 

2. The second way is related to content providers and gaining access to app internal files. I know this sounds crazy, but yes, it's possible to leak app internal files due to code mentioned above.

> [!tip]
> just keep in mind that if you see a pattern like this where the attacker can control an intent or part of an intent that is then being passed to startActivity, then this could lead to a serious issue.

### Mitigation 

If you are a developer, in order to fix this issue, ideally just never forward incoming attacker controlled intents in the first place, or use the Intent Sanitizer, which can be used to remove certain critical fields from an intent to limit the capabilities. Check this [Intent redirection  |  Security  |  Android Developers](https://developer.android.com/privacy-and-security/risks/intent-redirection).

# Returning Activity Results 






### References

[[Intents and Activities]]