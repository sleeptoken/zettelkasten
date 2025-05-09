
2025-05-07 14:15

Source: #reversing 

These are a lot of tools out there like [pyinstaller](https://github.com/pyinstaller/pyinstaller/wiki ), [py2exe](http://www.py2exe.org) and [bbfreeze](https://pypi.python.org/pypi/bbfreeze ) that will combine python scripts into stand alone executables packaged with all of the dependencies they need to run 

These packagers tend to create relatively large executables as they bundle up the Python interpreter, application code, python dependencies, and required native libraries into a single monolithic executable. For example, using PyInstaller to package a "Hello World" script will produce an executable approximately 8 MB in size. 

when we download the challenge file, it doesn't has a file type written, append `.exe` and open it 
- If the icon of the app wasn't a tip off, it should be apparent after some basic string analysis of the sample that we're dealing with Python. 
- Another clue that we're dealing with a packaged python script is the python library dependencies that are written to the file system

There are several tools out there that will attempt to extract the original files used to build Python packaged executables. In particular, [Pyinstaller Extractor](http://sourceforge.net/projects/pyinstallerextractor/files/?source=navbar ) by extremecoders seems to work nicely: 

Once you run the Pyinstaller Extractor tool on the executable, you'll notice a whole bunch of cruft that gets extracted. 
After a cursory survey of the files that are dropped, the "elfie" file seems pretty conspicuous with a ton of crazy looking [[py]] string variables. 
#### Deobfuscating the script 

It should be noted that it's not always possible to get the actual Python source extracted from the executable. Other Python packagers like py2exe only include the compiled byte code from the scripts.pyc files. So if you ever encounter that problem you'll have the additional problem of attempting to decompile the .pyc files. 

we have 56k lines of string variables of being declared then appear to be added together, base64 decoded, then exec'ed.

> This is a common obfuscation pattern you'll see in a variety of scripting languages where the "outer" script will simply build up a string to execute. Getting the "inner" script is as easy as changing the "exec" to a "print" and executing the code in a Python interpreter to dump out the inner script that it would have executed.

This inner script was actually programmatically obfuscated using a simple AST (Abstract Syntax Tree) obfuscator. 

### Shortcut

Once you ran the program, you could have just searched all memory for the ASCII string "flare-on" to find the key. Basically, the Python interpreter leaves a copy of the string in memory after the call to reversed('moc.no-eralf@OOOOY.sev0000L.eifle'), and you have a shot at finding it before the string gets garbage collected. You can try this from the memory map view in Olly, just right click on the first section and select "Search", then type "flare-on.com" in the ASCII field.

### References
Flare on 3 2015
 
[Flare-On-Challenges/Write-ups/2015/2015solution3.pdf at master · fareedfauzi/Flare-On-Challenges · GitHub](https://github.com/fareedfauzi/Flare-On-Challenges/blob/master/Write-ups/2015/2015solution3.pdf)

[Python Source Obfuscation using ASTs | Development & Security](https://jbremer.org/python-source-obfuscation-using-asts/)