
2024-6-19 12:36

This code demonstrates how to encrypt and decrypt data using **asymmetric encryption** with **Python** on a **Linux command line** environment. It utilizes Python's cryptographic capabilities to implement a secure encryption method, where a **public key** is used to encrypt data, and a corresponding **private key** is required for decryption.

To run in a terminal:
python enc.py "Your message to encrypt and decrypt"

### Generate Key Pair

```
private_key = rsa.generate_private_key(
    public_exponent=65537, 
    key_size=2048           
)
```

This code snippet generates an RSA private key with a key size of **2048 bits**. The **public_exponent 65537** is selected as it is a standard choice for RSA, offering an optimal balance between security and computational efficiency.

```
priv_pem = private_key.private_bytes(
    encoding=serialization.Encoding.PEM,                   
    format=serialization.PrivateFormat.TraditionalOpenSSL, 
    encryption_algorithm=serialization.NoEncryption()      
)
```

This code snippet converts the private key into a byte format suitable for file storage. It uses **PEM encoding**, a Base64-encoded format widely used for cryptographic keys, with no encryption applied.
Further I open a new file `priv.pem`, in binary write mode and save the serialized private key to this file.

```
public_key = private_key.public_key()
```

This snippet extracts the `public key` from the generated `private key`.

```
pub_pem = public_key.public_bytes(
    encoding=serialization.Encoding.PEM,                       
    format=serialization.PublicFormat.SubjectPublicKeyInfo     
)
```

This snippet transforms the public key to a `PEM-encoded` byte format, making it appropriate for file storage.
Further I open a new file, `pub.pem` in binary write mode and save the serialized public key to this file.

### Encrypting and Decrypting

The following code encrypts a message (`message`) using a public key, then decrypts it using the corresponding private key. It reads the key file paths from environment variables and uses `asymmetric encryption` (RSA) with `Optimal asymmetric encryption padding` (OAEP) to improve the process.

```
from cryptography.hazmat.primitives import serialization
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.asymmetric import padding
import sys, os
```

Imports the relevant modules: serialization, to manage key loading and format handling; hashes, to provides cryptographic hash functions (`SHA-256`); padding, to provide padding options for secure RSA encryption (`OAEP`); sys and os to Handle command-line arguments and environment variables.

```
if (len(sys.argv)) != 2:
    print ('Usage: ./enc.py message')
    exit(-1)
```

This code snippet verifies that exactly one command-line argument (the news alert message) is provided. If not, it displays usage instructions and exits.

```
org_alert = sys.argv[1].encode()
```

This line of code encodes the news alert message from the command-line argument to bytes, required for encryption.

```
pub_pem = os.environ.get('PUB_PEMK')
with open(pub_pem, 'rb') as pub_key_file:
    public_key = serialization.load_pem_public_key(pub_key_file.read())
```

This code snippet reads the public key file path from the environment variable `PUB_PEMK`; and then opens the public key file in binary mode, reads it, and loads it as a PEM-encoded public key using `serialization.load_pem_public_key`.

```
encrypted = public_key.encrypt(
    org_alert,
    padding.OAEP(
        mgf=padding.MGF1(algorithm=hashes.SHA256()),  
        algorithm=hashes.SHA256(),                   
        label=None                                  
    )
)
```

This code snippet encrypts `org_alert` using the `public key`; and then uses `OAEP` with `SHA-256` algorithm for both the main hash and the mask generation function 1 `MGF1`, which adds security by making the encryption include a factor of randomness, so as to produce different encryption outputs even if the same message is encrypted with the same key.

```
priv_pem = os.environ.get('PEMK')
with open(priv_pem, 'rb') as key_file:
    private_key = serialization.load_pem_private_key(
        key_file.read(),
        password=None
    )
```

This code snippet retrieves the private key file path from the environment variable `PEMK`; and opens the file in binary mode, reads it, and loads it as a PEM-encoded private key. The password parameter is set to None because the key is unecrypted.

```
decrypted = private_key.decrypt(
    encrypted,
    padding.OAEP(
        mgf=padding.MGF1(algorithm=hashes.SHA256()),  # Mask generation function with SHA-256
        algorithm=hashes.SHA256(),                   # Main hash algorithm for OAEP
        label=None                                   # Optional label, set to None
    )
)
```

This code snippet decrypts the encrypted message using the private key; and again uses `OAEP` padding with `SHA-256` for both the main hash algorithm and the `MGF1` function to ensure secure decryption.

```
print(decrypted.decode())
```

Finally, this line prints the decoded message in plaintext.


