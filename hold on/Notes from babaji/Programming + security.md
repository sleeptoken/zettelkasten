[29/09, 3:30 pm] +91 76190 25219: Mera USP hi “programmer + security” combo ka hai.
[29/09, 3:30 pm] +91 76190 25219: Pyramid, EY, Uptycs, Quick Heal, SentinelOne

Ye sab me yehi combo lga ke paise nikale hain
[29/09, 3:30 pm] +91 76190 25219: Cnergee was programmer + ops
[29/09, 3:31 pm] +91 76190 25219: Yup. Very rare.
[29/09, 3:31 pm] +91 76190 25219: Specially if you want someone who understands the system level things.
[29/09, 3:32 pm] +91 76190 25219: EY me mostly maine code review kiya. Backend codes.
[29/09, 3:32 pm] +91 76190 25219: Java, .NET dono stacks.
[29/09, 3:32 pm] +91 76190 25219: EY me AppScan tha. Not Veracode.
[29/09, 3:34 pm] +91 96996 17371: What do you think about Source Code Review now? Coz Veracode and other giants ne cheeze cover kar li. So is it still an option?
[29/09, 3:34 pm] +91 96996 17371: Recently Veracode introduced Code Fix using their LLM
[29/09, 3:34 pm] +91 76190 25219: Context specific chije tools se identity nahi hoti
[29/09, 3:35 pm] +91 76190 25219: Log tool lga dete hain. Context ka achaar dal lete hain.
[29/09, 3:36 pm] +91 76190 25219: Tool alert de dega, ki log me data disclose ho rha hai.

Fir bhai log report me likh denge.

Lekin ye dekhna bhul jayenge ki:

1. Wo sirf tab hai jab log level `TRACE` par set ho.
2. ⁠Default log level `TRACE` *nahi* hai
[29/09, 3:36 pm] +91 76190 25219: ab bhai log ko log levels bhi pata ho, usme thoda doubt hai mujhe
[29/09, 3:36 pm] +91 76190 25219: Similarly, crypto related issues nikalte hain
[29/09, 3:37 pm] +91 76190 25219: PRNG achcha use kiya, lekin entropy source hi chutiya hai.
[29/09, 3:37 pm] +91 76190 25219: Tool thode flag karega isko
[29/09, 3:37 pm] +91 76190 25219: .NET ka code hai, exceptions properly propagate nahi huye hain. Ya fir exceptions kafi deep me handle huye hain, and upper layers pe ghanta info nahi hai ki kya hua
[29/09, 3:38 pm] +91 76190 25219: Ya fir data race wali conditions ban rahi hain
[29/09, 3:38 pm] +91 76190 25219: Ya fir jaha data write ho raha hai, wo atomic hona chahiye tha.
[29/09, 3:38 pm] +91 76190 25219: But atomic nahi hai
[29/09, 3:38 pm] +91 76190 25219: ya fir file me write ke pahle lock acquire nahi kiya hai.
[29/09, 3:38 pm] +91 76190 25219: Ab ye sab chije context dependent hain.
[29/09, 3:39 pm] +91 76190 25219: Ye sab jgah nahi ghusana hota
[29/09, 3:39 pm] +91 76190 25219: lekin sab jgah skip bhi nahi karna hota.
[29/09, 3:39 pm] +91 76190 25219: code end user ki device pe chalega to uske constraints alag
[29/09, 3:39 pm] +91 76190 25219: hostile env me run hoga to uske constraints alag
[29/09, 3:39 pm] +91 76190 25219: backend me run hoga to uske alag
[29/09, 3:40 pm] +91 96996 17371: Hostile environment yaani?
End user samjha
Backend bhi samjh gaya
[29/09, 3:41 pm] +91 76190 25219: Jaha pe agla jan bujh ke tumhare sath bakchodi karega
[29/09, 3:41 pm] +91 76190 25219: Jaise school hai. Tumne bola covid hai, test online honge.
[29/09, 3:41 pm] +91 76190 25219: Students 100% exam system se bakchodi karenge
[29/09, 3:41 pm] +91 76190 25219: to wo tumhare pov se hostile env hua
[29/09, 3:41 pm] +91 96996 17371: Yaani Inside a debugger. Agar koi RE karne baith gaya
[29/09, 3:42 pm] +91 96996 17371: Toh code should detect it
[29/09, 3:42 pm] +91 96996 17371: And constraints change karna chahiye
[29/09, 3:42 pm] +91 76190 25219: Yup
[29/09, 3:42 pm] +91 76190 25219: ya gaming ka scene dekh lo
[29/09, 3:42 pm] +91 76190 25219: competitive gaming hai, to tumko anti-cheat chahiye hi
[29/09, 3:43 pm] +91 76190 25219: ab ye sab chije to tool se automatic flag nahi hone wali hain
[29/09, 3:43 pm] +91 76190 25219: waha pe manual karna padega
[29/09, 3:43 pm] +91 76190 25219: tool ka use code coverage figure out karne me karo
[29/09, 3:43 pm] +91 76190 25219: kya kidhar se call ho raha, kitni jgah se call ho raha
[29/09, 3:43 pm] +91 76190 25219: possible control flows kaun se hain
[29/09, 3:44 pm] +91 76190 25219: to EY wali job me yahi mera USP tha

[29/09, 3:44 pm] +91 76190 25219: kuchh clients ke liye hamne unki SDLC bhi theek karwayi
[29/09, 3:46 pm] +91 76190 25219: unit tests, integration tests, automated test harness, git hooks, mandatory PR checks (enforced by hosting server), various pipelines (to perform various checks etc.)
[29/09, 3:47 pm] +91 76190 25219: tum din bhar kaam karke sham ko sab commit kar ke gaye
[29/09, 3:47 pm] +91 76190 25219: overnight pipeline me sab hammer ho jayega
[29/09, 3:47 pm] +91 76190 25219: including source code audits
[29/09, 3:47 pm] +91 76190 25219: on changed code.
[29/09, 3:47 pm] +91 76190 25219: veracode, appscan etc integrate hote hain
[29/09, 3:47 pm] +91 96996 17371: Devsecops pipeline 👀
[29/09, 3:47 pm] +91 76190 25219: 1-2 hrs me sare checks complete, with a summarized report in your email
[29/09, 3:48 pm] +91 76190 25219: Can’t merge PR without resolving all the failing cases.