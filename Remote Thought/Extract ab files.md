
2024-11-05 15:38

Source: #android 

ab file = android backup file. Users create AB files using [[Android Debug Bridge]] (ADB)

```bash
( printf "\x1f\x8b\x08\x00\x00\x00\x00\x00" ; tail -c +25 backup.ab ) |  tar xfvz -
```

Replace **backup.ab** with the path to your file.
### References
https://app.hackthebox.com/challenges/Cat

https://stackoverflow.com/questions/18533567/how-to-extract-or-unpack-an-ab-file-android-backup-file