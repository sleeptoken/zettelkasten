
2024-06-15 17:00

Source: #privesc 

Tags: [[linux]] [[shell]]]

- Firstly List the programs your user is allowed to run via sudo using `sudo -l`
- For each program in the list, see if there is a shell escape sequence on GTFOBins (https://gtfobins.github.io/)
- If an escape sequence exists, run the program via `sudo` and perform the sequence to spawn a root shell.






### References
