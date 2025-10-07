Linkedin learning 

### windows callback routine 

In Microsoft, when a key on the keyboard is pressed, an event is signaled to the operating system.

Windows uses its keyboard driver to read the character that's been pressed and sends it as a message to the application that's waiting for it. However, Windows also allows other processors to look at the message as it passes through the system. That's how hotkeys work.
Windows does this by something known as a keyboard hook callback routine. It hooks the key that's been pressed and then calls back after processing it to pass it onto its original destination.
This technique can be used to write what's known as a simple keystroke logger