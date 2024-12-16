
2024-12-10 10:07

Source: #AOC24 

Tags: [[Forensics]]

The Atomic Red Team library is a collection of red team test cases that are mapped to the [MITRE ATT&CK](https://attack.mitre.org/) framework.
The library consists of simple test cases that can be executed by any blue team to test for detection gaps and help close them down.

start atom -   `Invoke-AtomicTest`
for help    -    `Get-Help Invoke-Atomictest`
then add a ATT&CK technique ID by googling it for your specific task

In the previous paragraph, we found multiple indicators of compromise through the Sysmon event log. (Found in windows event viewer) We can use this information to create detection rules to include in our EDR, SIEM, IDS, etc. These tools offer functionalities that allow us to import custom detection rules. There are several detection rule formats, including Yara, Sigma, Snort, and more.
- sigma rules should be written in `yaml`

### References
Day 4