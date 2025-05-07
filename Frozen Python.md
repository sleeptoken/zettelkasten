
2025-05-07 14:15

Source: #reversing 

These are a lot of tools out there like [pyinstaller](https://github.com/pyinstaller/pyinstaller/wiki ), [py2exe](http://www.py2exe.org) and [bbfreeze](https://pypi.python.org/pypi/bbfreeze ) that will combine python scripts into stand alone executables packaged with all of the dependencies they need to run 

These packagers tend to create relatively large executables as they bundle up the Python interpreter, application code, python dependencies, and required native libraries into a single monolithic executable. For example, using PyInstaller to package a "Hello World" script will produce an executable approximately 8 MB in size. 

when we download the challenge file, it doesn't has a file type written, append `.exe` and open it 
- If the icon of the app wasn't a tip off, it should be apparent after some basic string analysis of the sample that we're dealing with Python. 
- Another clue that we're dealing with a packaged python script is the python library dependencies that are written to the file system

out there that will attempt to extract the original files used to build Python packaged executables. In particular, Pyinstaller Extractor by extremecoders seems to work nicely: 
http://sourceforge.net/projects/pyinstallerextractor/files/?source=navbar 
Once you run the Pyinstaller Extractor tool on the executable, you'll notice a whole bunch of cruft that gets extracted. After a cursory survey of the files that are dropped, the "elfie" file seems pretty conspicuous with a ton of crazy looking Python string variables. 
Deobfuscating the script 
It should be noted that it's not always possible to get the actual Python source extracted from the executable. Other Python packagers like py2exe only include the compiled byte code from the scripts.pyc files. So if you ever encounter that problem you'll have the additional problem of attempting to decompile the .pyc files. In this case however, we lucked out and were able to grab the original source. 
Next, there is the problem of understanding the 56k lines of obfuscated source that appear to be adding random characters to strings that have variable names with annoying mix of zero's and capital o's.


### References
Flare on 3 2015
 
[Flare-On-Challenges/Write-ups/2015/2015solution3.pdf at master · fareedfauzi/Flare-On-Challenges · GitHub](https://github.com/fareedfauzi/Flare-On-Challenges/blob/master/Write-ups/2015/2015solution3.pdf)