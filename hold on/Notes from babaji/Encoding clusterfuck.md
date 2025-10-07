There is ascii. 

There is [[UTF-8]] which can have 1-4 bytes per “character”.  `H -> 48`

Then there is UTF-16, which takes two bytes per char. `H -> 00 48`

Then there are code pages.

Then there are cases where same character can be encoded in multiple ways.

Then there are concepts of graphemes, and grapheme clusters.

Then come ligatures.

- Question: how do you count size of string? Do you count bytes? Or characters? or graphemes?
- How do you even compare two graphemes? Same thing can be encoded in multiple ways.

Then comes Puny Code in Web coz bhau wanted domains in their own language
	This shit created Homograph attacks because Browser vendor thought RIR doing validation and RIR thought it was browser vendor

Same shit happened with Web too. 

Either you use
1) OS locale
2) Browser locale
3) Webpage defined encoding to decode it

Take windows. Then do this:

4. English version + Chinese language pack
5. ⁠Chinese version + english language pack
6. ⁠English version
7. ⁠Chinese version

All 4 cases will have subtle differences in encoding used to encode SAME data

Want to detect it at runtime? Windows will only tell you the active language; not the installed base language version

Now you do not know whether this is English build with Chinese pack, or Chinese build with no language packs.

In local or regional contexts and with improving technology, users might benefit from being able to use a wider range of characters; such use is not defined by this specification. 

Percent-encoded octets . . . may be used within a URI to represent characters outside the range of the US-ASCII coded character set if this representation is allowed by the scheme or by the protocol element in which the URI is referenced. Such a definition should specify the character encoding used to map those characters to octets prior to being percent-encoded for the URI.

even then it is TOO FUCKING EASY FOR WINDOWS
Now you get a million ways to deal with strings and it didn't include the endian thing after Unicode came
1. TSTR
2. ⁠LPCSTR
3. ⁠LPWSTR
4. ⁠LPSTR
5. ⁠LPTSTR

And probably some more.

Some API will take TSTR, other one will take LPWSTR, yet another one will take LPSTR
KEEP CONVERTING. WHY? BECAUSE FUCK YOU.

> Web mai they were using percent encoding for non US ASCII text but the question is ki isko %BF har codepage mai alag meaning diya hai toh kaunsa sahi ya kaunsa Galat

And kids, this is fucking why I do not want to deal with Windows.

##### ABI on Windows is still a mess.

Third party library download karni hai? Ab uske 15 build mil jayenge. Alag alag MSVC version ke liye pahle tum apna exact version dekho fir exact wahi version wala build configure karo Nahi to best case me linking error. Worst case me run time me data corruption, fir 2 hr bad crash.

Ab kar lo debug.
##### Linux? ABI for most part is stable.

CDN and DoH do not play very nicely
Also Puny Code uses Bootstring algorithm which is another nightmare

## comme over phy media

Comm over physical medium is total nightmare
Tumko tcp dikhta hai Uske niche dekho
- Shannon-Hartley theorem
- Noisy Channel Coding Theorem
- Error detection codes
- Error correction codes
- Setup and hold time violations
- Clock skew and jitter
- Self clocking signalling
- Self synchronizing signalling .....et-cetra

Physical medium hai. Electrical wires, electrical pulses. There will be cross talk.
And cross talk is bad. It is going to fuck your comm up in all 7 colors of rainbow
There is a reason why individual wires are shielded, the pairs are twisted across reach other.

> how does 2 different mediums collide and create problems ? 
> Data(1&0's) being transferred over Ethernet cable which is physical entity and frequencies like em and rf which are forms of energy collide and create problems like errors in data, malformed data, yeh bolne ke liye toh pata hai problem create hota hai but how ?

Only thing I could think of since antennas and wires are made of copper the wires might act antennas and pick up data

1s and 0s are indicated by different voltage levels, or in the case of twisted pairs, the voltage delta.
When you have a conductor in a changing electromagnetic field, it induces a voltage across the conductor.
Now the receiver doesn't know if the voltage is supposed to be 1.2V and 0.3V coming from the emf field, or the voltage is supposed to be 1.5V.

> Wouldnt the standards set for 802.3 state it ?

Aare ha, but suppose the standard says 1 is >= 1.5V, 0 is <= 1V
Sender sends 0.8V for 0.
But emf induces an additional 0.3V in the conductor.
Now receiver gets 1.1V and thinks it is 1.

Receiver ko kaise pata chalega ki 1.1V me se sender ne originally kitna bheja aur emf ki wajha se kitna change hua.
