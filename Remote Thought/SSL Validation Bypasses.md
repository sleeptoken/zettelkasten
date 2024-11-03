
2024-11-03 12:55

Source: #android 

A way to disable SSL validation and also to bypass certificate pinning is to use Frida to just disable it.
Now the way we bypass SSL pining on Android depends on how it's implemented. 
#### SSLContext & Network-Security-Config Bypass

- one of the implementation methods is to explicitly set up a key store with a single trusted certificate. This key store is then used to create a trust manager and that one will only validate this single certificate.

> - go to the android developer documentation and check the docs for `X509TrustManager` and see how the checkservertrusted method works 
##### *Lab*

Now let's start by using Frida Trace to find Invocations to the function check server trusted
```js
frida-trace -U -j '*!*checkServerTrusted*' FridaTarget
```

- on clicking button 1 we see that platform check server trusted is called and that it has quite a long signature. This will be the implementation that we will try to replace, but unfortunately the package name is missing here (the stuff that comes before the platform)

to get the package name Drop in the Frida repl using  `frida -U FridaTarget` then type - 
```js
Java.enumerateMethods("*Platform!*checkServerTrusted*")
```
In the results I can see that the package name is `com.android.org.conscrypt.Platform`.

now replace the `checkServerTrusted` Implementation by writing a script
```js
Java.perform(() => {
    var PlatformClass = Java.use("com.android.org.conscrypt.Platform");
    PlatformClass.chechServerTrusted.implementation = function(){
        console.log("check server trusted");
    }
})
```
on running our script and we get an error message. There are two different overloads for Check Server Trusted, and so we have to tell Frida which one we want to use.

Now, nicely enough, Frida shows us both available overloads and we can just copy paste one of this code in.

``` js
Java.perform(() => { 
	var PlatformClass = Java.use("com.android.org.conscrypt.Platform");          PlatformClass.checkServerTrusted.overload('javax.net.ssl.X509TrustManager', '[Ljava.security.cert.X509Certificate;', 'java.lang.String', 'com.android.org.conscrypt.AbstractConscryptSocket').implementation = function() { 
		console.log("Check server trusted"); 
	} 
})
```

another way to set up ssl pinning & validation is to use the network security config. And in the developer docs here you can see how a certificate might be pinned.
#### OKHTTP3 Bypass

I commonly used HTTP Library for Android is OKHTTP3 and it has its own support for certificate pinning and in the documentation we can see that there's an `OKHTTPClient.Builder()` and that one can be provided with a certificate pinner.

> And so to bypass certificate pinning,  we could just replace the certificate pinner function with our own implementation.

to find the full package name and class name for this OKHTTP builder  run `frida -U FridaTarget` then run -
```js
Java.enumerateMethods('*okhttp*Builder!*')
//('*okhttp*Builder!*') means filter for all OKHTTP classes that contain builder.
```

```js
Java.perform(() => {
	var BuilderClass = Java.use("okhttp3.OkHttpClient$Builder"); 
	BuilderClass.certificatePinner.implementation = function() { 
		console.log("Certificate pinner called"); 
		return this; // when we return this we are essentially doing nothing thereby disabling ssl pinner
	}
})
```

if the above script doesn't work that because we just disabled the SSL pinner but we didn't outright disable SSL validation so it turns out that OKHTTP3 will still use the trust manager

and so we can just run both our `X509TrustManager` script and our `OKHTTP3` script to bypass the pinning and now it works.
#### Bypassing SSL Pinning with Objection

```shell
objection explore
# will connect to our FRIDA target and then drop us into an objection CLI.

# once we get the shell type the following 
android sslpinning disable 
```

It's also able to bypass the network config pinning, but OKHTTP3 seems to not work.
Objection will probably fix the OKHTTP3 support, and so it might work on your machine.
### References

[[Dynamic Instrumentation]]
[Network security configuration  |  Security  |  Android Developers](https://developer.android.com/privacy-and-security/security-config)