[[win]]
Linkedin learning 
### Hidden history folder 

```
cd appdata\local\microsoft\windows
```

- List hidden files using `dir /ah`
The history sub directory does exists but we can't see it you just gotta `cd History` 

Once we are in we can see `desktop.ini` this is the method that Microsoft uses to hide the sub directory 

See the contents of desktop.ini using `type desktop.ini`. we see 2 clocking entries 
1. `CLSID` - This stops the subdirectory from being included in file based finds 
2. `UICLSID` - This stops the subdirectory from being seen using windows explorer 

### Alternate data streams

In the early `MS-DOS FAT` file systems the files were simply strings of data which could be read byte by byte by an application

NTFS files are a complex structure, at min these files contain a `$DATA` in it. 
	Which is where data read by an application resides, this is the data strean
NTFS can also have multiple data streams also known as alternate data stream 

We can see alternate data streams by using `/r` eg.`dir datafile.txt /r`

