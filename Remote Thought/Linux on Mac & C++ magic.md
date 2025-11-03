Installed Linux on macbook today.
%% How did SIP (System Integrity Protection) allow that?  %%
What do you mean? SIP is for MacOS partition. Wo alag hai
Naya partition banaya
usme Linux dala
So agar mai Mac me boot karunga to SIP will still be enabled
%% And and will sandboxing work in the same way in Linux? %%
No.
Linux ke time pe MacOS will not be running anywhere. One OS at a time

%% But, is that allowed? I mean mac does not have any restrictions on that? %%
Allowed? yes.
Apple shared documentation for hardware, so that Linux drivers can be written
The people who maintain Asahi figured out to make it work
The whole project is like reverse engineering stuff https://asahilinux.org/

%% Is it because of transition from Intel to Silicon chips? %%
PS: It's a driver issue. M3 ke aage apple ne documentation  of hardware hi public nahi kiya isliye driver kisine nahi likha for newer chipset
Asahi is for M1 and M2 only, M3 and M4 under dev hai
Even M1 / M2 pe bhi sab supported nai hai, fingerprint sensor nahi hai
Driver support is missing
Even audio is going to be hit or miss
specially HDMI audio
BT ka test karna padega
but that should work
All in all, Linux is not smooth sailing at all
Hence the dual boot. Because that personal itch
%% drivers mostly cpp mai likhte hai na? Asking coz me noob %%
C
Not C++
Because C does not have magic.
C++ on the other hand, is full of magic.
%% As in? %%
`A + B` can do any bloody thing behind the curtains; depending upon types of A and B, and how is `operator+` is defined.
Or, overloaded.
`A + B` sending an email in background because fuck you? Absolutely possible.
So you can’t reason about all the side effects, unless you dabble into a lot more corners of code base.
Which is a bad thing, when you are writing kernel space code.
When dealing in that corner, you better be able to reason about code
and keep things as much simple as you can
Otherwise, shit goes fubar real fucking quick.
Hardware gets fucked.
If you are particularly unlucky, hardware can get bricked permanently.
Ab ya to tum JTAG / SPI ke through uska firmware wapas flash karo
nahi to apni marwao
aur naya kharid ke laao