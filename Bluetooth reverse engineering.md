
2025-06-18 10:39

Source: #hardware 

Enable Bluetooth HCI snoop log from the Android Developer settings. HCI stands for host controller interface. it is essentially the communication between our host, which is android and our controller, which is the actual Bluetooth chip in the phone and vice versa. 
- This essentially creates a packet capture for us of all Bluetooth communication that was going on. 
### Loading the HCI snoop log in Wireshark

on modern android we couldn't just [[Android Debug Bridge]] pull these files.
use the following to generate a zip file, this will take a while.
```powershell 
adb bugreport
``` 
after extracting navigate to `FS-> data -> log -> bt` to find `btsnoop_hci.log`

- This isn't a text log instead it's a binary log containing full packet capture
### Lab

While reversing the hexlock app, we were understanding parts and renaming the variables and function.
there we stumble across 2 APIs.
1. API that takes username and password as json and returns an ID
2. API that return bluetooth ID and takes the above returned ID as a parameter

In order to hit those API's use [[curl]] 
```sh
curl -d '{"username":"stacksmash", "password":"test12"}' http://hexlock.hexoak.com:5001/user/locks -H 'Content-Type: application/json'
```
above command returns an array `[305,400]`
we use this to hit the `getbyid` API.
```sh
curl http://hexlock.hexoak.com:5001/padlocks/get-by-id/305
```
this returns a json object with `bluetooth_mac`, here we have the [[IDOR]] vulnerability, as we can iterate over the last `305` parameter and the API returns different `Bluetooth_mac` because of lack of authentication.

- see how the lock data is actually used in the app.

> Log messages are our biggest friend when reverse-engineering - They often contain original class names and information that helps us understand what a method or class is doing.

in the source code we can see lot of log messages have `BLELOCK` so we can assume that this class is `BLELOCK`, sometimes the compiler will inline other classes, in those cases we might see multiple different tags in the same class

# About Bluetooth

Bluetooth consists of a lot of different and big standards, and so this will be very simplified and just be enough to get started with reversing our application. Now, we have to split Bluetooth into essentially two categories
1. Classic Bluetooth 
2. Bluetooth low energy.
### Classic Bluetooth

Now, classic Bluetooth is nowadays mainly used for headphones and speakers, and pretty much everything else uses Bluetooth, low energy
### Bluetooth low energy

- Or BLE is very focused on low energy consumption. And so you can find devices that can run month or even years on a single battery charge using Bluetooth Low energy.
- BLE is also focused on a very fast connection setup. 
- Now, obviously, low power consumption also comes with a couple of drawbacks, and so BLE in general has a pretty low bandwidth, and so it's not really suitable for high bandwidth applications such as audio.
## Gatt (Generic Attribute Profile)

Now, almost all BLE devices use something called the `Generic Attribute Profile`. With gatt, we can query the device and ask what services and so-called characteristics. It provides. 
- for example, looking at a phone and a fitness tracker, then the phone would be the `Gatt` client and the fitness tracker would be the `Gatt` server. 
- our phone would ask the fitness tracker, Hey, what services do you provide?
- the fitness tracker would then reply with, Hey, I have a service for heart rate and I have a service for a running data.

There's a huge list of predefined services that can be provided by A BLE device, but you could also create a custom one. Each of the predefined services has a unique `UUID` assigned to it. This will be fairly important later on

as we continue reversing the Bluetooth part.

Now each service can expose so-called characteristics. Characteristics are essentially attributes that we can read and or write to, and we can also get notifications for when they change. 
In the case of the heart rate service, we would have 
- a characteristic for giving us the current heart rate,
- a characteristic for reading out the maximum heart rate,
- a characteristic for getting the resting heart rate. 
Both the heart rate service and the characteristics have assigned `UUIDs`, which you can lookup in the assigned numbers document from the Bluetooth body.
 



### References
[hextree.io](https://app.hextree.io/courses/android-bluetooth-reversing/bluetooth-basics)

UUID - https://stackoverflow.com/questions/13964342/android-how-do-bluetooth-uuids-work