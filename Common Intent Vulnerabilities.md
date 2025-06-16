
2025-06-12 12:38

Source: #android  
# Intent Redirect 

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

We can send an intent to a target application, but there's also the possibility to return results back to the calling app. 
#### Camera Access

##### Setup capture action

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

let's focus on the function call `startActivityForResult`. It takes an intent and the request code, which is not that important to us. It just helps match the request and response.
##### Getting the result 

The real question you should ask yourself is how do we get the result back? Well, for `startActivityForResult`, it will actually call this special function `onActivityResult`, which we can override in our activity. 

declare this outside `onCreate`
```java
@Override
protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent intent){
	super.onActivityResult(requestCode, resultCode, intent);
	Utils.showDialog(this,intent);
}
```

This function will be executed when a target application returns a result.

> When we press the button, we send the intent to declare our intention that we want to take a photo, and the operating system opens the camera app to handle this intent for us, we can now take a photo, and the camera app will now send the result back to our main activity, triggering the `onActivityResult` function. And we can see the returned intent. 

you can see that the returned intent doesn't contain much except for inline data. An extra field containing a Bitmap, an image. So the whole image that was taken got serialized into an intent sent to us. 

To view this in our app. extend our layout with an `ImageView` element. set the ID to `img` and then in the `onActivityResult` function we can get this view element, the `ImageView`, and set the image bitmap, grab the Bitmap from the intent.

This Bitmap is a complex object that got serialized or, in Android terms, it got parceled. So we have to use `getParcelableExtra` with the key name "data", and the returned value should be our Bitmap image object that we can now set to the `ImageView` in the layout.

append this code to `onActivityResult`
```java
Utils.showDialog(this,intent);  
Bitmap img = intent.getParcelableExtra("data");  
((ImageView) findViewById(R.id.img)).setImageBitmap(img);
```

the user can then decide to take a photo or not. If they decline, the returned intent will be empty. If they confirm, the intent contains the image data. No special permissions ever required for this app. 
# Hijack Implicit Intents

 Even if the target activity is intended to handle implicit intents like the camera app, we could still explicitly target this app, and we don't have to hope that an implicit intent resolves to that app. 
 
It should be pretty obvious when another application uses implicit intents, that our app could implement intent filters, that we become the target of those. Of course, in itself that is not a vulnerability. That's how the feature is intended to work. But it's another attack surface that we need to understand. 

For example, some apps might use implicit intents to pass security critical data, such as authentication tokens or private user data to another app if a malicious app can become the handler of that intent. The secret data leaks to the malicious app.

If an app sends an implicit intent to a malicious app and expects a response, the malicious app could return an evil intent. And you have to check whether the app handles the intent securely or not.

### Implement a response to an activity yourself. 

If your activity is intended to return a result back. So another activity starts your activity with a  `startActivityForResult`, you can return a result with `setResult`. This takes a result code which can be used to indicate whether it was the call was a success or a failure.
- And then we can also create an intent that gets returned back to the other app. Now this just sets the expected result,

1. but it might not trigger the other activity yet because our activity is still running. 
2. But we can also call `finish`, which indicates that the activity is done and should be closed, and then the activity result is propagated back to whoever launched our activity.




### References

[[Intents and Activities]]