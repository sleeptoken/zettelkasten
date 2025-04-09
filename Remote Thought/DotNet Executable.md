
2025-04-09 08:57

Source: #reversing 

Everything is carried out on [GitHub - mandiant/flare-vm: A collection of software installations scripts for Windows systems that allows you to easily setup and maintain a reverse engineering environment on a VM.](https://github.com/mandiant/flare-vm)

Throwing the exe into CFF Explorer we find out that it is a `.NET Binary`

Running the file shows a picture upon startup. Clicking on Decode button gives gibberish.

Run the file through `CodeTrack`. This shows that under `initializecomponent()` we see the Decode button is implemented and it calls `btnDecode_click`, this shows the code that is triggered when we click the button.

```
    this.pbRoge.Image = Resources.bob_roge;
	byte[] dat_secret = Resources.dat_secret;
	string text = "";
	for (int i = 0; i < dat_secret.Length; i++)
	{
		byte b = dat_secret[i];
		text += (char)((b >> 4 | ((int)b << 4 & 240)) ^ 41);
	}
	text += "\0";
	string text2 = "";
	for (int j = 0; j < text.Length; j += 2)
	{
		text2 += text[j + 1];
		text2 += text[j];
	}
	string text3 = "";
	for (int k = 0; k < text2.Length; k++)
	{
		char arg_B6_0 = text2[k];
		text3 += (char)((byte)text2[k] ^ 102);
	}
	this.lbl_title.Text = text3;
```
Further i was unable to find the `Resource.dat_secret` from `CodeTrack` 

Use `dnSpy` to explore the EXE and find the code that's triggered upon clicking the button, which is `Form1.btnDecode_Click()`. This loads `Resources.dat_secret.`

find that under Resources and right click and save it.
Use `HxD` to look at the contents

Rewrite the C code in python using chatGPT and print the final o/p
[My Adventure with Fireeye FLARE Challenge](https://parsiya.net/blog/2014-09-23-my-adventure-with-fireeye-flare-challenge/#ch1)

you get an email as a flag and you got to email them to receive an automated mail containing the next challenge 

### References
Flare-on 2014
[GitHub - fareedfauzi/Flare-On-Challenges: This repository aims to compile all Flare-On challenge binaries and write-ups. Update: 2014 -2024.](https://github.com/fareedfauzi/Flare-On-Challenges?tab=readme-ov-file)
[GitHub - junyian/flare-on-2014: Solutions to Flare-on 2014 challenges](https://github.com/junyian/flare-on-2014)
[My Adventure with Fireeye FLARE Challenge](https://parsiya.net/blog/2014-09-23-my-adventure-with-fireeye-flare-challenge/#ch1)