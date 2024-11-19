
2024-11-06 16:41

Source: #android 

documented in detail -> [[SSL Interception]]
current method shows a shortcut to setup a MITM for [[Cleartext Traffic Leads to Zip Path Traversal]]

visit the local proxy port (`127.0.0.1:8080`) in order to download the Burp certificate. 
And then let's head over to our Android emulator. I'm using here Android 14 without play services so we can get access to root. 
### Installing burp cert in system certs 

We copy over the Burp certificate and install it into the user certificate store. 

```powershell
PS C:\USER> adb root
PS C:\USER> adb shell 
```
script to inject our user certificates into the system certificate store of every app
```sh
# Create a separate temp directory, to hold the current certificates
# Otherwise, when we add the mount we can't read the current certs anymore.
mkdir -p -m 700 /data/local/tmp/tmp-ca-copy

# Copy out the existing certificates
cp /apex/com.android.conscrypt/cacerts/* /data/local/tmp/tmp-ca-copy/

# Create the in-memory mount on top of the system certs folder
mount -t tmpfs tmpfs /system/etc/security/cacerts

# Copy the existing certs back into the tmpfs, so we keep trusting them 
mv /data/local/tmp/tmp-ca-copy/* /system/etc/security/cacerts/

# Copy our new cert in, so we trust that to
cp /data/misc/user/0/cacerts-added/* /system/etc/security/cacerts/

# Update the perms & selinux context labels

chown root:root /system/etc/security/cacerts/*
chmod 644 /system/etc/security/cacerts/*
chcon u:object_r:system_file:s0 /system/etc/security/cacerts/*

# Deal with the APEX overrides, which need injecting into each namespace:

# First we get the Zveote process(es). which launch each app
ZYGOTE_PID=$(pidof zygote || true)
ZYGOTE64_PID=$(pidof zygote64 || true)
# N.b. some devices appear to have both!

# Apps inherit the Zygote's mounts at startup, so we inject here to ensure
# all newly started apps will see these certs straight away:

for Z_PID in "$ZYGOTE_PID" "$ZYGOTE64_PID"; do
	if [ -n "$Z_PID" ]; then
		nsenter --mount=/proc/$Z_PID/ns/mnt -- \
			/bin/mount --bind /system/etc/security/cacerts /apex/com.android.conscrypt/cacerts
	fi
done

# Then we inject the mount into all already running apps, so they
# too see these CA certs immediately:
# Get the PID of every process whose parent is one of the Zygotes:

APP_PIDS=$(
	echo "$ZYGOTE_PID $ZYGOTE64_PID" | \
	xargs -n1 ps -o 'PID' -P | \
	grep -v PID
)

# Inject into the mount namespace of each of those apps:
for PID in $APP_PIDS; do
	nsenter --mount=/proc/$PID/ns/mnt -- \
		/bin/mount --bind /system/etc/security/cacerts /apex/com.android.conscrypt/cacerts &
 done
 wait # Launched in parallel wait for completion here
 echo "System certificate injected"
```

### set up our proxy using the VPN Trick with the Rethink DNS app.

First, configure the DNS to use the system DNS

and then specify A-H-D-P-S Connect proxy pointing at the IP of our computer where Burp is running. Then that start the VPN and we should be ready to Intercept network request, which we can confirm using the web view test app. If this site doesn't load

and remains wide, your proxy set up falled somewhere. 

### References
