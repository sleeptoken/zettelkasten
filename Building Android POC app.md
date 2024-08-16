
2024-08-15 16:06

Source: #android

Tags: 
### Application Layout - to be able to display debug info in apps

Entry point is the code that is executed when the app is launched, this is defined in the android manifest config file in the activity tag referencing the activity class

- resources referenced in the `setcontentview` can be found in the `res > layout`
- on opening the `activity_main.xml` it opens a GUI editor like page (Design Page), we can switch to Text view by toggling the button on top right.

in the `design view > (RHS) Attributes ` give the `TextView` a unique identifier (need an id to reference in code) example - `home_text`

```java
TextView hometext = findViewById(R.id.home_text);  
hometext.setText("welcome guys");
//setText is the method that will change the text
```
As you can see the ID is not referenced by a string `"home_text"`, but as an identifier `R.id.home_text`. The resource reference class `R` is automatically generated based on various resources, including the IDs referenced in the layout .`xml` files.
#### References

[The Application Layout (hextree.io)](https://app.hextree.io/courses/first-android-app/building-a-clicker-app/the-application-layout)



