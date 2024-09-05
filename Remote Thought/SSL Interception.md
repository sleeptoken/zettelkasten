
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



### References
[Installing Certificate in User Store (hextree.io)](https://app.hextree.io/courses/network-interception/ssl-interception/installing-certificate-in-user-store)