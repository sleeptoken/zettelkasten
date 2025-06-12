
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

We can send an intent to a target application, but there's also the possibility to return results back to the calling app. let's explore this with an example by
#### Camera Access

You do not need to integrate with the camera library. Instead, We can use an intent.

And here is how the intent looks like. It's just an intent with an image capture action.
```java
public void onClick(View V){
	Intent intent = new Intent(); 
	intent.setAction("android.media.action.IMAGE_CAPTURE"); //implicit intent 
	startActivityForResult(intent,42);
}
```

can also be written as 
```JAVA
Intent intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
```

to be a bit more verbose we will not be using the constructor and instead call set action directly. 

in the intent we didn't set an explicit component to target. What you see here is called an implicit intent, where the Android system will help us find the target application to handle our action request to take a picture.





But more on that later. For now, let's focus on the function call startActivityForResult. It takes an intent and the request code,

which is not that important to us. It just helps match the request and response. Now you see that Android Studio tells

us that this method is deprecated. And if you want, you can look up in the developer documentation what the new way is.

But I still like to use this method for now because it still works. And the new method was only

introduced in Android nine. So this here works also below, but whichever you use it doesn't really matter. The real question you should ask yourself

is how do we get the result back? Well, for this old case with start activity for result, it will actually call

this special function onActivityResult, which we can override in our activity. This function will be executed when a target application returns a result.

And what is the returned result? Well, surprise, it's another intent. So let's add our utils.showDialog function we developed previously to see the details

of the intent that we receive back as a result. And now let's launch our app. When we press the button, we send the

intent to declare our intention that we want to take a photo, and the operating system opens the camera app.

To handle this intent for us, we can now take a photo, confirm the picture we have taken, and the camera app will now send the result

back to our main activity, triggering the onActivityResult function. And we can see the returned intent. And if you look closely,

you can see that this intent doesn't contain much except for inline data. An extra field containing a Bitmap, an image.

So the whole image that was taken got serialized into an intent sent to us. And we can now take this and display

it in our app. To do that, let's extend our layout with an ImageView element. I set the ID to image and then in the

onActivityResult function we can get this view element, the ImageView, and set the image bitmap to what. For that, we obviously need to

grab the Bilmap from the intent. And well, this Bitmap is a complex object that got serialized or, in Android terms, it got parceled.

So we have to use getParcelableExtra with the key name "data", and the returned value should be our Bitmap image object that we can now

set to the ImageView in the layout. If we now run the app, trigger the camera, snap a photo and confirm it,

the intent gets returned back and we can see the image in our UI. This example is, I think, really cool because it shows the power

of Android and the intent system. Our app did not require permissions to access the camera directly, nor did it require permissions to

access the photos stored on the phone. Instead, our app just declared the intention: "Hey, I'd like to take a photo."

The operating system redirected this to the camera, which opened, and the user can then decide to take a photo or not.

If they decline, the returned intent will be empty. If they confirm, the intent contains the image data. No special permissions ever required

for this app. And I think that's a really cool architecture. Now I'll have a look at the intent attack

surface application again and look at the flag. Do you expect the result? This flag highlights a peculiarity in the

information available in the activity when launched with startActivity versus startActivityForResult. It's a small weird thing to know about, but it led to a real vulnerability in

a Google app before. So it's definitely interesting to solve this flag. And of course the app has more flags

for you to get. So go ahead now and try to solve the flags listed below.

### References

[[Intents and Activities]]