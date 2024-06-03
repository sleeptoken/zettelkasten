
2024-06-03 12:43

Source: #tryhackme 

Tags: [[RSA]] [[Cryptography]]

 Let n be a +ve odd integer. Using Fermat's factorization method we can efficiently factorize n into p and q (n = p * q) provided the **difference between p and q is small**.

```
ssh-keygen -lf id_rsa.pub 
```
Show fingerprint of specified public key file along with the length 

```
ssh-keygen -e -m PKCS8 -f id_rsa.pub | openssl rsa -pubin -noout -modulus
```

```
echo "ibase=16; resulting_modules_from_prev_out | BC_LINE_LENGTH=0 bc
```







### References
[TryHackMe | Breaking RSA](https://tryhackme.com/r/room/breakrsa)