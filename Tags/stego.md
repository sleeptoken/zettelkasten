
#### Binwalk 
- Tool for searching binary images for embedded files and executable code
```sh
binwalk -e file.pdf  # extract files 
```

### Specifically for Images

The image bytes were modified such that we were only able to see half the image. We had to change the height of the image and reveal the full flag.
- just identify the resolution bytes and change the bytes that controlled the height.
- The “**05 F5"** bytes (1525 in decimal) here were controlling the height of the image. So I chose a random height greater than 1525 and converted it to hex. For example, 1700 in hex will be “**06 A4**”.



### Tools

[Aperi'Solve (aperisolve.com)](https://www.aperisolve.com/) for stego 

[Overlay images online](https://overlay.imageonline.co/) - Manipulate images 