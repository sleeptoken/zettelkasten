
2024-05-22 11:32

Source: #overthewire #bandit

Tags: [[linux]] [[ssh]] [[more]] [[shell]] [[vim]]

what shell is the default for a user, can be found at the end of the line for the user in the `/etc/passwd` file. user default shell is shown when using ssh .
`/etc/shells` shows all the shells that we have 

`more` is a shell command that allows the display of files in an interactive mode. Specifically, this interactive mode only works when the content of the file is too large to fully be displayed in the terminal window(shrink the window to use this feature). If the terminal is big enough then `more` will display everything and won't go into  interactive mode

It is possible to use vim to break out of a restricted environment and spawn a shell. 
interactive mode (`v`) - This command will open the file in the editor ‘vim’. 
once in vim 
	`:e` start vim in executable mode
	`:shell` to get a shell (uses default shell) use `:set shell=/bin/bash` before to get bash 

### References
[OverTheWire Bandit Level 25 -> 26 - Walkthrough - MayADevBe Blog](https://mayadevbe.me/posts/overthewire/bandit/level26/)