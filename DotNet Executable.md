
2025-04-09 08:57

Source: #reversing 

Everything is carried out on [GitHub - mandiant/flare-vm: A collection of software installations scripts for Windows systems that allows you to easily setup and maintain a reverse engineering environment on a VM.](https://github.com/mandiant/flare-vm)

Throwing the exe into CFF Explorer we find out that it is a `.NET Binary`

Running the file shows a picture upon startup. Clicking on Decode button gives gibberish.

Run the file through `CodeTrack`. This shows that under `initializecomponent()` we see the Decode button is implemented  

Use `dnSpy` to explore the EXE and find the code that's triggered upon clicking the button, which is Form1.btnDecode_Click(). This loads Resources.dat_secret. Found that resource in dnSpy and save as file. Then notice some algorithm being executed on the data. Replicate that in Python (see challenge1.py) and we can resolve it t


### References
Flare-on 2014
[GitHub - fareedfauzi/Flare-On-Challenges: This repository aims to compile all Flare-On challenge binaries and write-ups. Update: 2014 -2024.](https://github.com/fareedfauzi/Flare-On-Challenges?tab=readme-ov-file)
[GitHub - junyian/flare-on-2014: Solutions to Flare-on 2014 challenges](https://github.com/junyian/flare-on-2014)
[My Adventure with Fireeye FLARE Challenge](https://parsiya.net/blog/2014-09-23-my-adventure-with-fireeye-flare-challenge/#ch1)