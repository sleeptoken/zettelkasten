
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



### References
https://www.youtube.com/watch?v=jnBmI1eD-og
https://itsfading.github.io/posts/HexTree-Attack-Surface-App-Solutions/