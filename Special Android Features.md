
2024-08-17 18:41

Source: #android 

Tags: [[intent]]

``` java
Intent browserIntent = new Intent(Intent.ACTION_VIEW, Uri.parse("https://hextree.io/"));
startActivity(browserIntent); 
```
Activity creates the UI we see and so its farfetched to think that start activity will launch and display such an activity, but which activity will be shown depends on the parameter intent
what do we intent to open 
in the above code we intended to VIEW something and the data we pass is the URL 

add the above code after the `counter++;` inside an `if(counter==10)` statement
and after clicking on the button 10 times the app opens the browser and navigates to the URL we specified 
#### Gist 
we created an intent to view a URL and we start activity -> our app passes the intent to the android OS -> which now looks at the intent, sees that we want to view a URL, and android will start looking for installed apps that can fulfil our request, it'll find chrome browser app and hand over the intent to that. but how does the OS know that chrome can handle this? well it has to do with the android manifest 
in the android manifest file of chrome in activity-alias -> intent filter which says that it can handle actions to view the following protocols example. http, https etc. 




### References
