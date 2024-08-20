
2024-08-17 18:41

Source: #android 

Tags: [[intent]]

[[Building Android POC app]] sequel  
### Sending Intents

add the below code after the `counter++;` inside an `if(counter==10)` statement
and after clicking on the button 10 times the app opens the browser and navigates to the URL we specified 

``` java
Intent browserIntent = new Intent(Intent.ACTION_VIEW, Uri.parse("https://hextree.io/"));
startActivity(browserIntent); 
```

Activity creates the UI we see and so its farfetched to think that start activity will launch and display such an activity, but which activity will be shown depends on the parameter intent
what do we intend to open ? 
in the above code we intended to VIEW something and the data we pass is the URL 
#### Gist 

1. we created an intent to view a URL and we start activity 
2. our app passes the intent to the android OS 
3. which now looks at the intent, sees that we want to view a URL, and android will start looking for installed apps that can fulfil our request, 
4. it'll find chrome browser app and hand over the intent to that. 
5. but how does the OS know that chrome can handle this? 
6. well it has to do with the android manifest 
7. in the android manifest file of chrome in activity-alias -> intent filter which says that it can handle actions to view the following protocols example. http, https etc. 
### Receiving Intents

create a new empty view activity(name it `NewActivity`) by going to `new > activity > empty view activity`  
this automatically adds new activity tag in the android manifest file 
in order to receive intents we need to expose the activity by changing "false" to "true"
> `android:exported="true"` - ***while reverse engineering keep an eye out for this***

```xml
<activity
	android:name=".NewActivity"
	android:exported="true">
	<intent-filter>  
	    <action android:name="android.intent.action.SEND" />  
	    <data android:mimeType="text/plain"/>  
	    <category android:name="android.intent.category.DEFAULT" />  
	</intent-filter>
</activity>
```

- `android:exported="true"`  ->   adding this will tell that we can only handle sent intents
- `data android:mimeType="text/plain"`   ->   where the included data is just plain text 
- `category android:name="android.intent.category.DEFAULT" ` ->   we use the category default. this means the app is happy to present itself to handle this kind of data as default app 

Further create a text like in [[Building Android POC app]] and give it the id of `debug_text`
add the following code to the `NewActivity.java` file
```java
Intent receivedIntent = getIntent();  // access the incoming intent 
String sharedText = receivedIntent.getStringExtra(Intent.EXTRA_TEXT);  
if(sharedText!=null){  
    TextView debugText = findViewById(R.id.debug_text);  
    debugText.setText("Shared: "+sharedText);  
}
```
#### How will we receive intents

when you select a text and choose to share, our app will show up in the android sharesheet because of the code in the `intent-filter`
#### Attack surface

the fact that we have an exposed activity handling an incoming intent. this is a attack surface in itself. 
### References

android manifest of chrome - [chrome/android/java/AndroidManifest.xml - chromium/src - Git at Google (googlesource.com)](https://chromium.googlesource.com/chromium/src/+/b71e98cdf14f18cb967a73857826f6e8c568cea0/chrome/android/java/AndroidManifest.xml#156)