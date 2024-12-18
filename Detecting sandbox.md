
2024-12-18 10:22

Source: #AOC24 

Tags:

There are different types of anti-malware techniques used by malwares 

we are checking a technique, which checks if the directory `C:\Program Files` is present by querying the Registry path
This directory is often absent on sandboxes or other virtualized environments, which could indicate that the malware is running in a sandbox.

registry check function 
```c
void registryCheck() {
    const char *registryPath = "HKLM\\Software\\Microsoft\\Windows\\CurrentVersion";
    const char *valueName = "ProgramFilesDir";
    
    // Prepare the command string for reg.exe
    char command[512];
    snprintf(command, sizeof(command), "reg query \"%s\" /v %s", registryPath, valueName);
    // Run the command
    int result = system(command);
    // Check for successful execution
    if (result == 0) {
        printf("Registry query executed successfully.\n");
    } else {
        fprintf(stderr, "Failed to execute registry query.\n");
    }
}
int main() {
    const char *flag = "[REDACTED]";
    registryCheck();
        return 0;

} 
```

Yara rule can detect malware just by checking for a specific string that appears when we query the registry 

Yara rule to catch the previous c code 
```javascript
rule SANDBOXDETECTED
{
    meta:
        description = "Detects the sandbox by querying the registry key for Program Path"
        author = "TryHackMe"
        date = "2024-10-08"
        version = "1.1"

    strings:
        
    $cmd= "Software\\Microsoft\\Windows\\CurrentVersion\" /v ProgramFilesDir" nocase

    condition:
        $cmd
}
```

to bypass this yara rule we use obfuscation 

```c
void registryCheck() {
// Encoded PowerShell command to query the registry
    const char *encodedCommand = "RwBlAHQALQBJAHQAZQBtAFAAcgBvAHAAZQByAHQAeQAgAC0AUABhAHQAaAAgACIASABLAEwATQA6AFwAUwBvAGYAdAB3AGEAcgBlAFwATQBpAGMAcgBvAHMAbwBmAHQAXABXAGkAbgBkAG8AdwBzAFwAQwB1AHIAcgBlAG4AdABWAGUAcgBzAGkAbwBuACIAIAAtAE4AYQBtAGUAIABQAHIAbwBnAHIAYQBtAEYAaQBsAGUAcwBEAGkAcgA=";
    // Prepare the PowerShell execution command
    char command[512];
    snprintf(command, sizeof(command), "powershell -EncodedCommand %s", encodedCommand);

    // Run the command
	int result = system(command);
    ..//same as before.....
```

other tools include floss 
this works similar to the strings command in Linux but is optimized for malware 
### Sysmon 

**Sysmon**, a tool from Microsoft's Sysinternals suite, continuously monitors and logs system activity across reboots. This Windows service provides detailed event data on process creation, network connections, and file changes—valuable insights when tracing malware behaviour.
### References
Day 6 