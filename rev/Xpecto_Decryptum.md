# 🧙‍♂️ Xpecto Decryptum

> **Challenge Name:** Xpecto Decryptum  
> **Category:** Reverse  
> **Challenge Type:** Rev / Crypto  
> **File:** ```theypackedme.py```  

---

## 📜 Description

> *"Someone thought it’d be funny to wrap a secret inside layers of Python magic. With just the right touch, you might just unwrap this digital present and find the treasure inside. But be careful—messing with the wrong parts might leave you scratching your head instead!"*

> This challenge presents a Python script that uses Fernet encryption to obfuscate and protect an embedded payload. Our task was to decrypt the payload without executing it blindly and extract the hidden flag safely.

---

## 🔍 Challenge File

`theypackedme.py` contains encrypted data and the logic to decrypt and execute it:

```python
import base64
from cryptography.fernet import Fernet

payload = b'<encrypted_data>'
key_str = 'supersecretkeyforthexpecto2025XD'
key_base64 = base64.b64encode(key_str.encode())
f = Fernet(key_base64)
plain = f.decrypt(payload)
exec(plain.decode())
```

---

## ⚠️ The Problem

The use of exec() poses a security risk — we don’t want to execute unknown code directly. Instead, we must safely inspect the decrypted contents.

---

## 🔓 Solution Steps
   
### 1. Understand the Encryption Scheme:
   ◦ Payload is Fernet-encrypted.
   ◦ Key is a string, base64-encoded for Fernet compatibility.

### 2. Avoid exec():
   ◦ Replace exec(plain.decode()) with print(plain.decode()) to examine the payload.

### 3. Decrypt Safely:

```python
import base64
from cryptography.fernet import Fernet

# Inputs from the original script
payload = b'gAAAAABn2vH52liBfuFfazSkLcAVO5N3AaoIJslqczD_GbOi7Ygbq2eKZWF2YYqFSUv34O-eQeH_2DvL-cJZceUmk1Tlp_fOEhXn30uG4tjmgZm0Dfc2LhSDJtFrdYSriHuKOIfpjrs0V6xyo8_Vg2HsUePmXDXFF_mzicZqDX5wLw0NHBFn3fqjsdbavOnqZw0YIhPbEbyaupbzOULllF9VUGvn53D3RFlyHD8M3jY8xAPkj0YY6D2WmmKtDu16XKK4asdLEsYo-7uSGFG-UO6fxdf1ncWXD8pM0EAOYj1Y6tlSH3t9qXE='
key_str = 'supersecretkeyforthexpecto2025XD'

# Prepare key
key_base64 = base64.b64encode(key_str.encode())
f = Fernet(key_base64)

# Decrypt and print
plain = f.decrypt(payload)
print(plain.decode())
```

---

## 🏁 Flag

```css
saic{pr1n7ing_ju57_unp4ck3d_my_s3cr3t}
```
