
2024-08-15 16:06

Source: #android

Tags: 
### Application Layout - updating text elements in the UI

Entry point is the code that is executed when the app is launched, this is defined in the android manifest config file in the activity tag referencing the activity class

- resources referenced in the `setcontentview` can be found in the `res > layout`
- on opening the `activity_main.xml` it opens a GUI editor like page (Design Page), we can switch to Text view by toggling the button on top right.

in the `design view > (RHS) Attributes ` give the `TextView` a unique identifier (need an id to reference in code) example - `home_text`

```java
TextView hometext = findViewById(R.id.home_text);  
hometext.setText("welcome guys"); //show text in the app ui
//setText is the method that will change the text
```
As you can see the ID is not referenced by a string `"home_text"`, but as an identifier `R.id.home_text`. The resource reference class `R` is automatically generated based on various resources, including the IDs referenced in the layout .`xml` files.
#### References
[The Application Layout (hextree.io)](https://app.hextree.io/courses/first-android-app/building-a-clicker-app/the-application-layout)

### Handling Button Clicks

when developing POC attack apps, it often makes sense to add a button or multiple buttons to trigger the attack 

- in the layout editor we find multiple components, including a button. drag it into the component tree 
- adjust the layout constraints in the constraint widget 
- change the id as we need it later to reference the button in code 

add the following in the text view 
```java
Button hommeButton = findViewById(R.id.home_button);  
hommeButton.setOnClickListener(new View.OnClickListener() {  
    @Override  
    public void onClick(View view) {  //code that gets executed on each click
        Log.i(tag:"HEXTREE", msg:"WELCOME from the button");  //print some text to logcat
    }  
});
```
#### References
[Handling Button Clicks (hextree.io)](https://app.hextree.io/courses/first-android-app/building-a-clicker-app/handling-button-clicks)
### Decompiling apps

we use `jadx` for decomplication 
- `.apk` - this is a file that contains all the other files necessary to install and run our app
- when publishing on play store we upload a `.aad(Android App Bundle)` these bundles are like `.apks`, but contain a lot more information. this allows the play store to generate optimized `.apks` for users. 
> look at android manifest files first cause that tells us about the exposed attack surface 