if u are given a screenshot of a [hexdump](https://ctftime.org/writeup/40273) of a zip file, then use an online [OCR](https://www.onlineocr.net/) tool to extract the text from the image.

Then using `xxd` convert the dump back to binary and get the zip file

```sh
xxd -r -p a screenshot.zip && 7z e screenshot.zip
```