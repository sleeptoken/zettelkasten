 
2024-11-29 21:13

Source: #android 

Tags: [[Intent Attack surface app]]

The development documentation for [Activity](https://developer.android.com/reference/android/app/Activity) explains it like this:

> _"An activity is a single, focused thing that the user can do. Almost all activities interact with the user, so the Activity class takes care of creating a window for you in which you can place your UI"_

When you open an app on your phone, any screen you can see has an Activity behind it, and it turns out that other apps can sometimes interact with these activities as well. Generally one activity implements one screen in an app.  

> [!tip]
> Always take the apk in jadx and see the android manifest, check the activities, click on them and see the code. 
> Then target the intent using android studio

# Attack Surface

In order to attack an app, we need to understand what can we even interact with. That's why we should look at the `AndroidManifest.xml` and look for any `<activity>` with the `android:exported="true"` attribute.

`android:exported="true"` tells us weather other apps can interact with this app directly or not 

`android:exported="true"` is interesting to us ,`android:exported="false"` can be ignored
# Intent

Here is how the official documentation describes [Intent](https://developer.android.com/reference/android/content/Intent):

> _"An intent is an abstract description of an operation to be performed."_

But a more intuitive description could be: _"Declaring an intention to do something, and let Android figure out the app that can handle it"_

# Starting Activities

#### Basic exported activity

Activities are responsible to render the screen of an app. So if your app has multiple screens, you can use `startActivity()` to start another activity. To do that you have to create an Intent object and target a specific activity class.

the `SecondActivity` cannot be started from another app. Only the app itself can start this "Internal" activity
we can start a non exported (exported="false") activity from WITHIN the same app 
- how we generally prepare intents to call other apps

```java
Intent intent = new Intent();
intent.setComponent(new ComponentName("io.hextree.activitiestest", "io.hextree.activitiestest.SecondActivity"));
startActivity(intent);
```

An alternative syntax to specify the target package and activity is also: **Typical Internal Intent** 
typically code you see inside of apps starting their own internal activities

```java
Intent intent = new Intent();
intent.setClassName("io.hextree.activitiestest", "io.hextree.activitiestest.SecondActivity");
startActivity(intent);
```

An app can always start any of its own activities, but in order to allow other apps to start your activity, they have to be exported. There is also one exported activity by default, which is the "launcher activity" that is used as a main entry point into the app. This allows the home screen or launcher application to start your app when you click it.
##### Using adb shell

```powershell
adb shell am start-activity -n io.hextree.attacksurface/io.hextree.attacksurface.activities.Flag1Activity
```
#### Intent with extras 

actions are mentioned in the android manifest then use -a tag in the adb shell command 

# Incoming Intent

The intent object that was used to start an activity, is available to the app via `getIntent()`.This feature is used to pass data to other apps, and thus it becomes a major attack surface.
#### Intent Attack surface

- `Activity.getIntent()`
- `onActivityResult(int requestCode, int resultCode, Intent data)`
- `onNewIntent(Intent data)`

we can add things to an intent by using `intent.putExtra("hextree",1337);`
# Journey of an Intent 

#### Parcels

[[Parcels]] are a way to serialize and de-serialize objects. We are serializing the intent object, which contains target activity string and lots of different values

Android documentation definition - 

A parcel is a container for a message that can be sent through IBinder. A parcel can contain flattened data that will be unflattened on the other side of the IPC. So it's similar in concept to an intent. Intents are also just like a message, but they are higher level Java objects. parcels are really low level involved in the actual inter process communication between the different apps. 
#### Binder

Binder is a kernel driver. It sits deep in the Android operating system. 
If you access your phone or emulator with ADB and search for anything binder related using
```sh
find / 2>/dev/null | grep binder
```

There are binder related device files. These are fancy Linux files that actually expose the driver and you have native libraries that implement the interaction with these.

Now, if you run the apps on a phone or emulator with root access, you can also check the processes of your apps. 
```sh
su root
ps -A | grep hextree
```

So first of all, you see here the Android apps are running as their own Linux process. 

```sh
cat /proc/2417(process-id)/maps
```
With the process id, we can find the memory map of this app process. This app lots of stuff into memory and if you look through it, you will find `libbinder`,
> the native code library implementing the interaction with the binder driver, which allows the app to send data to other app process such as the intent when we call start activity.

Luckily we don't really have to interact with any of the low level stuff directly. That's the beauty of Android and the development SDK.
# Implementing Intent Debug Features

We can get an incoming intent in our activity with `getIntent()` and we could simply print the two string of it to the logs.

```java
Intent intent = getIntent();
Log.i("Hextree","intent"+intent.toString());
```
In the output, we can see that things like the intent, action and category and flags are there, but it's not nicely formatted and it doesn't contain everything, especially no extra values.

#### Helper Function for intent data

So when working on an intent-related attack, I like to use a helper function to display all the data stored in an intent. Create a new class called `Utils.java`. 

-- updated code listed below -- 

And now back in our main activity we can call `Utils.dumpIntent` and log it.

```java
Intent intent = getIntent();
Log.i("Hextree","intent"+Utils.dumpIntent(intent));
```
If we now launch the app, we get the launch intent and we can see the details in logcat. The output is much more verbose. 

It could be improved a lot. For example, the flags is an Integer value, but in reality it's individual bits that are set or not set making up that integer.
example - `Intent.Flags`
	decimal: 268435456
	bin: 0001000000000000000000
there is one bit set 

So we could create a function that actually decodes the flags into a string. - Which bits are set?

Also, an intent could include an extra with another Intent (`Extra_Intent`), a nested intent. And in that case you would like to dump that intent value as well. And when it's nested, we would like to add some Indentation to the output.
#### Improved Helper Function

Replace the `Utils.java` with the following, What you get with this is now a more powerful dump Intent function, which for example 
- calls getFlagsString, which checks the set bits of the flag integer and creates a string with all the set flags. 
- there's also another useful function called showDialog, which will dynamically build a layout with different elements to create a dialog, pop up with all the intent details, so you can conveniently add it into any activity you might want to display an intent.
  
```java
// package io.hextree.activitiestest;

import android.animation.AnimatorSet;
import android.animation.ObjectAnimator;
import android.app.Dialog;
import android.content.Context;
import android.content.Intent;
import android.graphics.Color;
import android.graphics.Typeface;
import android.os.Bundle;
import android.view.Gravity;
import android.view.ViewGroup;
import android.view.Window;
import android.view.WindowManager;
import android.widget.Button;
import android.widget.LinearLayout;
import android.widget.TextView;

import java.util.Set;

public class Utils {
    public static String dumpIntent(Context context, Intent intent) {
        return dumpIntent(context, intent, 0);
    }

    private static String dumpIntent(Context context, Intent intent, int indentLevel) {
        if (intent == null) {
            return "Intent is null";
        }

        StringBuilder sb = new StringBuilder();
        String indent = new String(new char[indentLevel]).replace("\0", "    ");

        // Append basic intent information
        sb.append(indent).append("[Action]    ").append(intent.getAction()).append("\n");
        // Append categories
        Set<String> categories = intent.getCategories();
        if (categories != null) {
            for (String category : categories) {
                sb.append(indent).append("[Category]  ").append(category).append("\n");
            }
        }
        sb.append(indent).append("[Data]      ").append(intent.getDataString()).append("\n");
        sb.append(indent).append("[Component] ").append(intent.getComponent()).append("\n");
        sb.append(indent).append("[Flags]     ").append(getFlagsString(intent.getFlags())).append("\n");


        // Append extras
        Bundle extras = intent.getExtras();
        if (extras != null) {
            for (String key : extras.keySet()) {
                Object value = extras.get(key);
                if (value instanceof Intent) {
                    sb.append(indent).append("[Extra:'").append(key).append("'] -> Intent\n");
                    // Recursively dump nested intents with increased indentation
                    sb.append(dumpIntent(context, (Intent) value, indentLevel + 1));  
                } else if (value instanceof Bundle) {
                    sb.append(indent).append("[Extra:'").append(key).append("'] -> Bundle\n");
                    // Recursively dump nested intents with increased indentation
                    sb.append(dumpBundle((Bundle) value, indentLevel + 1));
                } else {
                    sb.append(indent).append("[Extra:'").append(key).append("']: ").append(value).append("\n");
                }
            }
        }

        // Query the content URI if FLAG_GRANT_READ_URI_PERMISSION is set
        /*
        if ((intent.getFlags() & Intent.FLAG_GRANT_READ_URI_PERMISSION) != 0) {
            Uri data = intent.getData();
            if (data != null) {
                sb.append(queryContentUri(context, data, indentLevel + 1));
            }
        }
        */

        return sb.toString();
    }
    
    public static String dumpBundle(Bundle bundle) {
        return dumpBundle(bundle, 0);
    }

    private static String dumpBundle(Bundle bundle, int indentLevel) {
        if (bundle == null) {
            return "Bundle is null";
        }

        StringBuilder sb = new StringBuilder();
        String indent = new String(new char[indentLevel]).replace("\0", "    ");

        for (String key : bundle.keySet()) {
            Object value = bundle.get(key);
            if (value instanceof Bundle) {
                sb.append(String.format("%s['%s']: Bundle[\n%s%s]\n", indent, key, dumpBundle((Bundle) value, indentLevel + 1), indent));
            } else {
                sb.append(String.format("%s['%s']: %s\n", indent, key, value != null ? value.toString() : "null"));
            }
        }
        return sb.toString();
    }

    private static String getFlagsString(int flags) {
        StringBuilder flagBuilder = new StringBuilder();
        if ((flags & Intent.FLAG_GRANT_READ_URI_PERMISSION) != 0) flagBuilder.append("GRANT_READ_URI_PERMISSION | ");
        if ((flags & Intent.FLAG_GRANT_WRITE_URI_PERMISSION) != 0) flagBuilder.append("GRANT_WRITE_URI_PERMISSION | ");
        if ((flags & Intent.FLAG_GRANT_PERSISTABLE_URI_PERMISSION) != 0) flagBuilder.append("GRANT_PERSISTABLE_URI_PERMISSION | ");
        if ((flags & Intent.FLAG_GRANT_PREFIX_URI_PERMISSION) != 0) flagBuilder.append("GRANT_PREFIX_URI_PERMISSION | ");
        if ((flags & Intent.FLAG_ACTIVITY_NEW_TASK) != 0) flagBuilder.append("ACTIVITY_NEW_TASK | ");
        if ((flags & Intent.FLAG_ACTIVITY_SINGLE_TOP) != 0) flagBuilder.append("ACTIVITY_SINGLE_TOP | ");
        if ((flags & Intent.FLAG_ACTIVITY_NO_HISTORY) != 0) flagBuilder.append("ACTIVITY_NO_HISTORY | ");
        if ((flags & Intent.FLAG_ACTIVITY_CLEAR_TOP) != 0) flagBuilder.append("ACTIVITY_CLEAR_TOP | ");
        if ((flags & Intent.FLAG_ACTIVITY_FORWARD_RESULT) != 0) flagBuilder.append("ACTIVITY_FORWARD_RESULT | ");
        if ((flags & Intent.FLAG_ACTIVITY_PREVIOUS_IS_TOP) != 0) flagBuilder.append("ACTIVITY_PREVIOUS_IS_TOP | ");
        if ((flags & Intent.FLAG_ACTIVITY_EXCLUDE_FROM_RECENTS) != 0) flagBuilder.append("ACTIVITY_EXCLUDE_FROM_RECENTS | ");
        if ((flags & Intent.FLAG_ACTIVITY_BROUGHT_TO_FRONT) != 0) flagBuilder.append("ACTIVITY_BROUGHT_TO_FRONT | ");
        if ((flags & Intent.FLAG_ACTIVITY_RESET_TASK_IF_NEEDED) != 0) flagBuilder.append("ACTIVITY_RESET_TASK_IF_NEEDED | ");
        if ((flags & Intent.FLAG_ACTIVITY_LAUNCHED_FROM_HISTORY) != 0) flagBuilder.append("ACTIVITY_LAUNCHED_FROM_HISTORY | ");
        if ((flags & Intent.FLAG_ACTIVITY_CLEAR_WHEN_TASK_RESET) != 0) flagBuilder.append("ACTIVITY_CLEAR_WHEN_TASK_RESET | ");
        if ((flags & Intent.FLAG_ACTIVITY_NEW_DOCUMENT) != 0) flagBuilder.append("ACTIVITY_NEW_DOCUMENT | ");
        if ((flags & Intent.FLAG_ACTIVITY_NO_USER_ACTION) != 0) flagBuilder.append("ACTIVITY_NO_USER_ACTION | ");
        if ((flags & Intent.FLAG_ACTIVITY_REORDER_TO_FRONT) != 0) flagBuilder.append("ACTIVITY_REORDER_TO_FRONT | ");
        if ((flags & Intent.FLAG_ACTIVITY_NO_ANIMATION) != 0) flagBuilder.append("ACTIVITY_NO_ANIMATION | ");
        if ((flags & Intent.FLAG_ACTIVITY_CLEAR_TASK) != 0) flagBuilder.append("ACTIVITY_CLEAR_TASK | ");
        if ((flags & Intent.FLAG_ACTIVITY_TASK_ON_HOME) != 0) flagBuilder.append("ACTIVITY_TASK_ON_HOME | ");
        if ((flags & Intent.FLAG_ACTIVITY_RETAIN_IN_RECENTS) != 0) flagBuilder.append("ACTIVITY_RETAIN_IN_RECENTS | ");
        if ((flags & Intent.FLAG_ACTIVITY_LAUNCH_ADJACENT) != 0) flagBuilder.append("ACTIVITY_LAUNCH_ADJACENT | ");
        if ((flags & Intent.FLAG_ACTIVITY_REQUIRE_DEFAULT) != 0) flagBuilder.append("ACTIVITY_REQUIRE_DEFAULT | ");
        if ((flags & Intent.FLAG_ACTIVITY_REQUIRE_NON_BROWSER) != 0) flagBuilder.append("ACTIVITY_REQUIRE_NON_BROWSER | ");
        if ((flags & Intent.FLAG_ACTIVITY_MATCH_EXTERNAL) != 0) flagBuilder.append("ACTIVITY_MATCH_EXTERNAL | ");
        if ((flags & Intent.FLAG_ACTIVITY_MULTIPLE_TASK) != 0) flagBuilder.append("ACTIVITY_MULTIPLE_TASK | ");
        if ((flags & Intent.FLAG_RECEIVER_REGISTERED_ONLY) != 0) flagBuilder.append("RECEIVER_REGISTERED_ONLY | ");
        if ((flags & Intent.FLAG_RECEIVER_REPLACE_PENDING) != 0) flagBuilder.append("RECEIVER_REPLACE_PENDING | ");
        if ((flags & Intent.FLAG_RECEIVER_FOREGROUND) != 0) flagBuilder.append("RECEIVER_FOREGROUND | ");
        if ((flags & Intent.FLAG_RECEIVER_NO_ABORT) != 0) flagBuilder.append("RECEIVER_NO_ABORT | ");
        if ((flags & Intent.FLAG_RECEIVER_VISIBLE_TO_INSTANT_APPS) != 0) flagBuilder.append("RECEIVER_VISIBLE_TO_INSTANT_APPS | ");

        if (flagBuilder.length() > 0) {
            // Remove the trailing " | "
            flagBuilder.setLength(flagBuilder.length() - 3);
        }

        return flagBuilder.toString();
    }

    public static void showDialog(Context context, Intent intent) {
        if(intent == null) return;
        // Create the dialog
        Dialog dialog = new Dialog(context);
        dialog.requestWindowFeature(Window.FEATURE_NO_TITLE);
        dialog.setCancelable(true);

        // Create a LinearLayout to hold the dialog content
        LinearLayout layout = new LinearLayout(context);
        layout.setOrientation(LinearLayout.VERTICAL);
        layout.setPadding(20, 50, 20, 50);
        layout.setBackgroundColor(0xffefeff5);


        // Add a TextView for the title
        TextView title = new TextView(context);
        title.setText("Intent Details: ");
        title.setTextSize(16);
        title.setTextColor(0xff000000);
        title.setTypeface(Typeface.DEFAULT, Typeface.BOLD);
        title.setPadding(0, 0, 0, 40);
        title.setGravity(Gravity.CENTER);
        title.setBackgroundColor(0xffefeff5);
        layout.addView(title);

        // Add a TextView for the message
        TextView message = new TextView(context);
        message.setText(dumpIntent(context, intent));
        message.setTypeface(Typeface.MONOSPACE);
        message.setTextSize(12);
        message.setTextColor(0xff000000);
        message.setPadding(0, 0, 0, 30);
        message.setGravity(Gravity.START);
        message.setBackgroundColor(0xffefeff5);
        layout.addView(message);

        // Add an OK button
        Button positiveButton = new Button(context);
        positiveButton.setText("OK");
        positiveButton.setTextColor(0xff000000);
        positiveButton.setOnClickListener(v -> dialog.dismiss());
        layout.addView(positiveButton);

        // Set the layout as the content view of the dialog
        dialog.setContentView(layout);

        // Adjust dialog window parameters to make it fullscreen
        Window window = dialog.getWindow();
        if (window != null) {
            window.setLayout(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT);
            window.setBackgroundDrawableResource(android.R.color.transparent);
            WindowManager.LayoutParams wlp = window.getAttributes();
            wlp.gravity = Gravity.BOTTOM;
            wlp.flags &= ~WindowManager.LayoutParams.FLAG_DIM_BEHIND;
            window.setAttributes(wlp);
        }

        dialog.show();
        // Animate the dialog with a slide-in effect
        layout.setTranslationY(2000); // Start off-screen to the right
        layout.setAlpha(0f);
        ObjectAnimator translateYAnimator = ObjectAnimator.ofFloat(layout, "translationY", 0);
        ObjectAnimator alphaAnimator = ObjectAnimator.ofFloat(layout, "alpha", 1f);
        AnimatorSet animatorSet = new AnimatorSet();
        animatorSet.playTogether(translateYAnimator, alphaAnimator);
        animatorSet.setDuration(300); // Duration of the animation
        animatorSet.setStartDelay(100); // Delay before starting the animation
        animatorSet.start();
    }
}
```
  
Back in the main activity, we can now replace the log output with a call to Utils.showDialog.
```java
Intent intent = getIntent();
Utils.showDialog(this,intent);
```

If we now run the app, we get the launch intent and the dialog will show the intent details displaying us all the details of the intent that was used by the launcher to launch our main activity.

> [!important]
> The most common intent attack surface is the `onCreate()` method which handles the incoming intent from `getIntent()`. But the [activity lifecycle](https://developer.android.com/guide/components/activities/activity-lifecycle) is a bit more complex and the `Flag7Activity` [[Intent Attack surface app]] highlights another possible threat surface involving the `onNewIntent(Intent intent)` method. 
> 

# Activity vs. Service vs. Receiver

Understanding the different ways how app-to-app communication works on Android is important. 

Activities are the main entry point of an app, and they are responsible for rendering the UI layout that you can see. So generally, everything you can see is an activity. And these activities can be exposed in the Android manifest so other apps can start them. 

A service can perform long-running operations in the background. It does not provide a user interface. Once started, a service might continue running for some time, even after the user switches to another application. 

Broadcast receivers are a little bit different. Android apps can send or receive broadcast messages. Many of them come from the Android system, but it could also be an interaction with other regular apps. Usually, these broadcasts are sent when some kind of event occurs. broadcast receivers are short. You notify them in the background, they do something and they finish again. 

| **Component**          | **UI Presence**       | **Declared like so** (in android manifest) | **Purpose**                                              | **Lifecycle**                                | **Example Use Case**                           | **Receives Intents** |
| ---------------------- | --------------------- | ------------------------------------------ | -------------------------------------------------------- | -------------------------------------------- | ---------------------------------------------- | -------------------- |
| **Activity**           | Yes (Main UI element) | `<activity>`                               | Displays UI and handles user interaction                 | Runs while user is interacting with it       | Viewing a photo, entering login details        | Yes                  |
| **Service**            | No                    | `<service>`                                | Performs long-running background tasks                   | Can run long after app is in background      | Uploading files, playing music in background   | Yes                  |
| **Broadcast Receiver** | No                    | `<receiver>`                               | Responds to system-wide or app-specific broadcast events | Short-lived; stops after handling the intent | Responding to "airplane mode on", SMS received | Yes                  |
# Explicit vs Implicit Intent 

Explicit Intent 
```java
Intent intent = new Intent();
intent.setClassName("io.hextree.attacksurface","io.hextree.attacksurface.activites.FlagActivity");
startActivity(intent);
```

Implicit Intent
```java
Intent intent = new Intent();
intent.setAction("android.media.action.IMAGE_CAPTURE");
startActivity(intent);
```
Inside many `<activity>` tags you can often also find `<intent-filter>`. These intent filters serve an important role in the resolution of implicit intent.
- android looks through all the apps to find the one that can handle this (specified action). 
- If your app creates an implicit intent with a specific action, category or data URI, the Android system can look through the intent filters and find a matching app.

You can see this in action by adding 
```java
intent.addFlags(Intent.FLAG_DEBUG_LOG_RESOLUTION);
```
# Pending Intents

PendingIntents are involved when implementing widgets or notifications, 

Think again about the vulnerability class of intent redirects. For example, 
- application A creates an intent and packages it as an extra intent within another intent (`intent.putExtra("EXTRA_INTENT",innerintent)`) and sends it to application B, and then 
- application B takes the extra intent and calls `startActivity(innerIntent)` with it.

> This is an intent redirect, and the cool thing here is that the redirected intent runs with the permission of application B,

which makes sense because application B calls start activity with this intent, and this can be abused if application B has some kind of non-exported activities or other permissions Application A cannot use directly 

pending intents.

- Application A(malicious) an create an intent and place it inside of a PendingIntent object, and then send this one over to Application B `(PendingIntent pendingIntent = PendingIntent.getActivity(this,0,targetIntent,PendingIntent.FLAG_MUTABLE);`.
 - Application B can then take the PendingIntent object and execute it or send it. But unlike with the startActivity from before, the executed PendingIntent now runs with the permission of Application A.
 
> Application A delegated the execution of the intent to Application B, so in some way it's a security mitigation of intent redirects. The redirected intent will always only have the permission of the original application A this way a malicious app A cannot reach anything bad it couldn't before. Application B executes this intent with the permission of Application A. 

- However, think about the case when the roles are reversed. Let's imagine App A sends a PendingIntent to a malicious App B. 
Now B receives the permission to perform an action with the permission of Application A. This is a cool feature, but could also lead to some issues.

there's also the concept of immutable and mutable PendingIntents. If it is a mutable PendingIntent, the receiving application can modify the PendingIntent before using it. 
### References
[Intent Attack Surface](https://app.hextree.io/courses/intent-threat-surface/intents-and-activities)
