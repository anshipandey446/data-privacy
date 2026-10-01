

 Data Privacy Practical – Cryptography

# DATA PRIVACY PRACTICAL

 ## Experiment No. 1

 ### Title

 **Study and Implementation of Cryptographic Techniques**

 ### Aim

 To study and implement basic cryptographic techniques such as **encryption, hashing, and digital signatures** using Python.

---

 ## Objectives

 - To understand the basic concept of cryptography.
- To understand how encryption protects private data.
- To generate a hash of data using SHA-256.
- To understand the use of digital signatures.
- To learn how cryptography is used in data privacy and security.

---

 ## Introduction

 **Cryptography** is a technique used to protect information from unauthorized access. It converts data into a secure form so that only authorized users can access or verify it.

 Three important cryptographic techniques are:

 - **Encryption:** Protects the confidentiality of data.
- **Hashing:** Helps check whether data has been changed.
- **Digital Signature:** Helps verify the authenticity and integrity of data.

---

 # Part A: Encryption and Decryption

 ## Theory

 Encryption changes readable data, called **plaintext**, into an unreadable form called **ciphertext**.

 Decryption converts the ciphertext back into the original message.

 **Plaintext → Encryption → Ciphertext → Decryption → Plaintext**

 For this practical, we use the **Fernet** encryption method.

 ## Requirements

 - Python 3
- `cryptography` library

 Install the library using:

```
pip install cryptography
```

 ## Algorithm

 1. Import the Fernet module.
2. Generate an encryption key.
3. Create a cipher using the key.
4. Enter a message.
5. Encrypt the message.
6. Decrypt the encrypted message.
7. Display the original, encrypted, and decrypted messages.

 ## Program

```
from cryptography.fernet import Fernet

# Generate key
key = Fernet.generate_key()

# Create cipher
cipher = Fernet(key)

# Original message
message = "Hello, this is private data"

# Encrypt message
encrypted = cipher.encrypt(message.encode())

print("Encrypted:", encrypted)

# Decrypt message
decrypted = cipher.decrypt(encrypted)

print("Decrypted:", decrypted.decode())
```

 ## Output

```
Encrypted: b'gAAAAA...'
Decrypted: Hello, this is private data
```

 The encrypted value may be different every time the program runs.

 ## Result

 The message was successfully encrypted and decrypted.


 # Part B: Hashing

 ## Theory

 Hashing converts data into a fixed-length value called a **hash**.

 We use **SHA-256** in this practical.

 A small change in the original data produces a different hash. Therefore, hashing can be used to check **data integrity**.

 Unlike encryption, hashing is not normally used to recover the original message.

 ## Algorithm

 1. Import the `hashlib` module.
2. Enter a message.
3. Apply the SHA-256 algorithm.
4. Generate the hash value.
5. Display the hash.

 ## Program

```
import hashlib

# Message
message = "Hello World"

# Generate SHA-256 hash
hash_value = hashlib.sha256(message.encode()).hexdigest()

print("Message:", message)
print("SHA-256 Hash:", hash_value)
```

 ## Output

```
Message: Hello World
SHA-256 Hash: a591a6d40bf420404a011733cfb7b190...
```

 The complete hash will contain 64 hexadecimal characters.

 ## Result

 The given message was successfully converted into a SHA-256 hash.

---

 # Part C: Digital Signature

 ## Theory

 A **digital signature** is used to verify the authenticity and integrity of digital data.

 A digital signature uses two keys:

 - **Private key** – used to create the signature.
- **Public key** – used to verify the signature.

 ## Algorithm

 1. Generate a private key.
2. Generate the corresponding public key.
3. Enter a message.
4. Create a digital signature using the private key.
5. Verify the signature using the public key.
6. Display the verification result.

 ## Program

```
from cryptography.hazmat.primitives.asymmetric import rsa
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.asymmetric import padding

# Create private key
private_key = rsa.generate_private_key(
    public_exponent=65537,
    key_size=2048
)

# Create public key
public_key = private_key.public_key()

# Message
message = b"Hello World"

# Create digital signature
signature = private_key.sign(
    message,
    padding.PKCS1v15(),
    hashes.SHA256()
)

print("Digital Signature Created")

# Verify signature
try:
    public_key.verify(
        signature,
        message,
        padding.PKCS1v15(),
        hashes.SHA256()
    )

    print("Signature is valid")

except:
    print("Signature is invalid")
```

 ## Output

```
Digital Signature Created
Signature is valid
```

 ## Result

 The digital signature was successfully created and verified.

---

 # Part D: Checking Modified Data

 This program demonstrates what happens when the original message is changed after signing.

```
modified_message = b"Hello Python"

try:
    public_key.verify(
        signature,
        modified_message,
        padding.PKCS1v15(),
        hashes.SHA256()
    )

    print("Signature is valid")

except:
    print("Signature is invalid - Data was changed")
```

 ## Output

```
Signature is invalid - Data was changed
```

 ## Result

 When the message was modified, the digital signature verification failed. This shows that digital signatures can help detect changes to signed data.

---

 # Comparison

 | Technique | Purpose | Example |
| --- | --- | --- |
| Encryption | Protect confidential data | Fernet |
| Hashing | Check data integrity | SHA-256 |
| Digital Signature | Verify authenticity and integrity | RSA |

---

 # Applications of Cryptography

 Cryptography is used in many real-world applications, such as:

 - Secure messaging
- Online banking
- Password protection
- Secure websites
- Digital documents
- Online payments
- Data storage
- Authentication systems

---

 # Advantages

 - Protects confidential information.
- Helps prevent unauthorized access.
- Helps detect changes to data.
- Provides authentication.
- Supports secure communication.

---

 # Conclusion

 In this practical, we studied and implemented three basic cryptographic techniques: **encryption, hashing, and digital signatures**.

 Encryption was used to protect the confidentiality of a message. SHA-256 hashing was used to generate a hash value for checking data integrity. RSA digital signatures were used to create and verify the authenticity and integrity of a message.

 Therefore, cryptography is an important part of **data privacy and cybersecurity**.

