[04/10, 12:37 am] +91 76190 25219: Installed Linux on macbook today.
[04/10, 12:49 am] +91 91368 97339: How did SIP allow that? 
[04/10, 12:51 am] +91 96996 17371: System Integrity Protection
[04/10, 12:52 am] +91 76190 25219: What do you mean? SIP is for MacOS partition. Wo alag hai
[04/10, 12:52 am] +91 91368 97339: And and will sandboxing work in the same way in Linux?
[04/10, 12:52 am] +91 76190 25219: Naya partition banaya
[04/10, 12:52 am] +91 76190 25219: usme Linux dala
[04/10, 12:52 am] +91 76190 25219: So agar mai Mac me boot karunga to SIP will still be enabled
[04/10, 12:52 am] +91 76190 25219: No.
[04/10, 12:53 am] +91 76190 25219: Linux ke time pe MacOS will not be running anywhere. One OS at a time
[04/10, 12:53 am] +91 91368 97339: But, is that allowed? I mean mac does not have any restrictions on that?
[04/10, 12:53 am] +91 90043 02809: The people who maintain Asahi figured out to make it work
[04/10, 12:53 am] +91 90043 02809: The whole project is like reverse engineering stuff
[04/10, 12:54 am] +91 90043 02809: https://asahilinux.org/
[04/10, 12:56 am] +91 76190 25219: Allowed? yes.
[04/10, 12:56 am] +91 76190 25219: Apple shared documentation for hardware, so that Linux drivers can be written
[04/10, 12:59 am] +91 96996 17371: Is it because of transition from Intel to Silicon chips?

PS: It's a driver issue. M3 ke aage apple ne documentation  of hardware hi public nahi kiya isliye driver kisine nahi likha for newer chipset
[04/10, 1:02 am] +91 76190 25219: Asahi is for M1 and M2 only
[04/10, 1:02 am] +91 76190 25219: M3 and M4 under dev hai
[04/10, 1:03 am] +91 76190 25219: Even M1 / M2 pe bhi sab supported nai hai
[04/10, 1:03 am] +91 76190 25219: fingerprint sensor nahi hai
[04/10, 1:03 am] +91 81497 91460: hai to, m1 mai fingerprint sensor
[04/10, 1:04 am] +91 81497 91460: oh you mean driver support for it
[04/10, 1:04 am] +91 76190 25219: Yes. Driver support is missing
[04/10, 1:06 am] +91 76190 25219: even audio is going to be hit or miss
[04/10, 1:06 am] +91 76190 25219: specially HDMI audio
[04/10, 1:06 am] +91 76190 25219: BT ka test karna padega
[04/10, 1:06 am] +91 76190 25219: but that should work
[04/10, 1:06 am] +91 76190 25219: All in all, Linux is not smooth sailing at all
[04/10, 1:06 am] +91 76190 25219: Hence the dual boot. Because that personal itch
[04/10, 1:07 am] +91 96996 17371: 👀 drivers mostly cpp mai likhte hai na? Asking coz me noob
[04/10, 1:07 am] +91 76190 25219: C
[04/10, 1:07 am] +91 76190 25219: Not C++
[04/10, 1:07 am] +91 76190 25219: Because C does not have magic.
[04/10, 1:07 am] +91 76190 25219: C++ on the other hand, is full of magic.
[04/10, 1:07 am] +91 96996 17371: As in?
[04/10, 1:08 am] +91 76190 25219: `A + B` can do any bloody thing behind the curtains; depending upon types of A and B, and how is `operator+` is defined.
[04/10, 1:08 am] +91 76190 25219: Or, overloaded.
[04/10, 1:08 am] +91 76190 25219: `A + B` sending an email in background because fuck you? Absolutely possible.
[04/10, 1:08 am] +91 76190 25219: So you can’t reason about all the side effects, unless you dabble into a lot more corners of code base.
[04/10, 1:09 am] +91 76190 25219: Which is a bad thing, when you are writing kernel space code.
[04/10, 1:09 am] +91 76190 25219: When dealing in that corner, you better be able to reason about code
[04/10, 1:09 am] +91 76190 25219: and keep things as much simple as you can
[04/10, 1:10 am] +91 76190 25219: Otherwise, shit goes fubar real fucking quick.
[04/10, 1:10 am] +91 76190 25219: Hardware gets fucked.
[04/10, 1:11 am] +91 76190 25219: If you are particularly unlucky, hardware can get bricked permanently.
[04/10, 1:11 am] +91 76190 25219: Ab ya to tum JTAG / SPI ke through uska firmware wapas flash karo
[04/10, 1:11 am] +91 76190 25219: nahi to apni marwao
[04/10, 1:11 am] +91 76190 25219: aur naya kharid ke laao