
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

# Threat Model Notes

When an app sends cleartext `http://` traffic we can probably consider this to be a security issue. However traffic that can only be intercepted with a purposely installed certificate is not an issue. By installing a CA we are intentionally "weakening" the security of the device to allow us to decrypt the traffic.


### References
[Installing Certificate in User Store (hextree.io)](https://app.hextree.io/courses/network-interception/ssl-interception/installing-certificate-in-user-store)