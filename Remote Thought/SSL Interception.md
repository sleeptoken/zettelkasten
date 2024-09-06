
2024-09-04 15:19

Source: #android 

Tags: 
## Installing Certificate in User Store

In order to be able to intercept TLS/SSL communication, we need the certificate of our proxy tool to be trusted by the device. Via the android settings we can easily install a certificate into the "user" CA store.

User certificates are only trusted by apps when:

1. The app targets Android 6 (API level 23) or lower
2. Or [network security config](https://developer.android.com/privacy-and-security/security-config) specifically includes "user" certs

Example `xml/network_security_config.xml` which trusts "user" certificates:

```xml
<base-config cleartextTrafficPermitted="false">
    <trust-anchors>
        <certificates src="system" />
        <certificates src="user" />
    </trust-anchors>
</base-config>
```
### Installing process

- setup proxy manually for Wi-Fi like in [[Internet perms & Networking]] 
- then go to `<machine-ip>:<port>` and download burps certificate `<machine-ip>` is the IP of the machine on which burp is configured
- search for certificate in settings and go to CA certificate, install the certificate u just downloaded 

> That's important because by installing a user cert, we are intentionally weakening the security of our phone. If you install a CA certificate, the certificate owner could intercept your data such as passwords or credit card details. Whoever has this CA can perform MITM attacks on your phone. 
#### On older Android versions (Android 11 - API level 30)  

The cert might be grayed out. That's because it expects another file format. In that case, you have to follow some basic steps to convert it, and then you can install the ca.
- take the file on a `linux` machine and run the following 
```bash
openssl x509 -inform DER -in cacert.der -out cacert.pem
```
- import the file to our system and install the cert 
### Useful logcat errors

if we try to intercept traffic of another app like google translate or the weather app, nothing happens or even we get errors. I wanted to mention that errors can be very helpful if you keep an eye on logcat. The apps might print out valuable information, maybe even the full URLs that you are interested in, so it can actually make sense to provoke as SSL errors. Intentionally. It can be a neat trick, but of course we want to intercept traffic,
### how come it doesn't work despite having the cert installed?

The reason for that lies in the trusted credentials settings. Here we can see there are system certificates and user certificates. System certificates are the pre-installed trusted certificate authorities, and our installed certificate doesn't show up here. Ours got installed into the user added certificates 
So our burp suite ports certificate is a **second class certificate.** It's Not fully trusted, but then why did it work in Chrome?
The reason for this lies in Chrome's network security config, applications can specify a security config in the Android manifest. Chrome explicitly trusts user and system certificates 

which means if we look at an app that doesn't have the setting, the app will fall back to certain default values in the Android documentation.

So if an app doesn't specify network security config, by default - 
- if the app targets Android 9 or later 
	- clear text traffic is not permitted and only system certificates are allowed
- Before Android 9, starting with Android 7
	- clear text traffic was allowed, but it still trusted only system certificates 
- Only with Android 6 and before 
	- apps trusted user credentials by default. 
### Custom SSL verification 

- For example, we could try to install our certificate as a system certificate. Maybe that works.
- Or we have to resort to modifying and patching. our target APK using `apktool` and add a network security config to trust also user certificates.
-  Or maybe we have to even do some dynamic Instrumentation. eg. frida.

## Installing Certificate in System Store

if we install the certificate from a proxy tool, it is considered a "user" certificate
from the android documentation we know that by default apps only trust "system" certificates.
in order to install a cert as a system certificate we need to be root but we get a root shell directly on a phone w google services unless the device is rooted
### Rooting android 

ways to get root access - 
1. root a physical android device (do this on a spare android device)
2. rooting the google play emulator (lots of tutorials about this on YouTube)
3. using a non-google emulator image (download an emulator w/o google services)
#### Setting up the device

- in the device manager of android studio. We can create a new emulator, but we need to make sure we select one that doesn't have Google Play installed
> - But not only that, there has been a major change in the way how certificates are handled between Android 13 and Android 14 (A14 blocks modification of system certificates, even as root) .This method only works for Android 13 and before.
 
- go to x86 image and select Tiramisu API 33 Android 13.0 (default android system image)

This is clean Android with any of the special Google services installed, there's not even the Chrome browser,

So let's quickly set up the proxy stuff, going into the settings, install a certificate, I drag and drop the Burp certificate onto the phone and then we can find it in the downloads folder. And after installation we can confirm this. Here is the user certificate and of course we want to disable LTE

`adb root` - to restart adb as root
`adb shell` - to get the root shell

```shell
ls -lah /data/misc/user/0/cacerts-added/
```
here's the folder that contains the user added certificates

```shell
ls -lah /system/etc/security/cacerts
```
here's the folder w system certificates

```shell
cp /data/misc/user/0/cacerts-added/9a5ba575.0 /system/etc/security/cacerts
```
Can we just copy our certificate into that folder? Unfortunately, no. The file system is mounted to read only, but there's a trick.

1. Ensure you are root (`adb root`), and execute the following commands in `adb shell`:

```bash
# Backup the existing system certificates to the user certs folder
cp /system/etc/security/cacerts/* /data/misc/user/0/cacerts-added/

# Create the in-memory mount on top of the system certs folder
mount -t tmpfs tmpfs /system/etc/security/cacerts

# copy all system certs and our user cert into the tmpfs system certs folder
cp /data/misc/user/0/cacerts-added/* /system/etc/security/cacerts/

# Fix any permissions & selinux context labels
chown root:root /system/etc/security/cacerts/*
chmod 644 /system/etc/security/cacerts/*
chcon u:object_r:system_file:s0 /system/etc/security/cacerts/*
```
#### Downloading apps on the rooted device 

This android system doesn't have any google apps Installed, but we can use our other emulator with installed play services and a Google account to install, for example, Google Translate
download google translate from the play store 
```bash
pm list packages -f | grep translate 
```
with PM packages -f. We can find the path to the translate APK, 
then we can use a ADB to pull download the APK onto our machine so we can then ADB install it onto our new emulator. 

By the way, if you have multiple emulator instances running, you have to use `-t` with the transport ID to specify which device you wanna interact with. 

###### Transport ID 
if we have multiple devices connected then we can address them with the transport ID `-t`
```powershell
D:\tmp\proxy> adb devices -l
List of devices attached 
emulator-5554    device product:..:..:..:1
emulator-5556    device product:..:..:..:2
```


And if we did everything right,

04:23

we can have a look at bur rún, translate, enter some text, and the API requests will show up. In the HDP history,

04:30

we just successfully performed SSL interception on a Google app that only trusts system certificates. Now before we move on, just a few words on

04:39

this specific example. If you look closely in the HDP log, then you can see that the API requests contain an API key.

04:48

This API key is necessary to be able to use the translate. API Google offers an official way how you can use Google translate with your own API key.

### References
[Installing Certificate in User Store (hextree.io)](https://app.hextree.io/courses/network-interception/ssl-interception/installing-certificate-in-user-store)