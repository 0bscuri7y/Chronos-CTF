# 🔐 CTF Writeup – "Extra Salt, Admin Spilled" (Crypto Challenge)

> **Challenge Name:** Response  
> **Category:** Crypto  
> **File:** ```server.py```

## 🧠 Challenge Description

> Our ultra-secure auth system adds extra salt to every login. But guess what? Drop a magic byte, and the server spills the admin's hash like an overfilled salt shaker. 🤦‍♂️  
> Mix it with a fresh pinch of salt, fake the login, and boom—flag served! 🍽️😏

### 🛰️ Server Info
Host: 
```yaml
nc iitmandi.co.in 8096
```

---

## 🧵 TLDR

We exploit a **logic flaw** in the login process:  
The server reveals the internal `admin_hash` and `session_salt` when sent a special byte `0x7f`.  
We use the leaked hash and generate the correct response to bypass the login and capture the flag.

---

## 🔍 Vulnerability Analysis

### Server Logic Summary

- On connect: generates a random `session_salt`
- Computes:
  ```python
  admin_hash = SHA3_256(admin_pass + session_salt)
  ```
If a client sends the byte 0x7f, it leaks:
```
admin_hash

session_salt
```

Login Flow
When admin attempts to log in:

Server prompts for:

```bash
Compute hash(admin_hash + '<login_salt>'):
```

Then verifies:
```python
expected = SHA3_256(admin_hash + login_salt)
```

---

## ⚠️ Vulnerability

If the client knows admin_hash and sees the login_salt, it can compute the final login hash locally—no password needed!

---

## 🧪 Step-by-Step Exploit

> Step 1 – Leak Admin Hash

Send the special byte 0x7f to leak internal values:
```bash
printf '\x7f\n' | nc iitmandi.co.in 8096
```
Output:
```vbnet
admin: 8d26faff9bc1fa8fe90295dfbd78bdcab79bc585fe5ce86ab00d69cd64f6802f
Secret salt: 'tfP9dV0UKvAaKJLFcNebBgo40uWXZwzu'
```

> Step 2 – Begin Admin Login
Type:

```bash
1
```
Then:

```bash
admin
```
You'll be prompted:

```bash
Compute hash(admin_hash + 'ydsh3JL1nhyY7KMzNxtLCihmYNBQ1wQ0'):
```

Step 3 – Locally Compute the Hash
```python
import hashlib

admin_hash = "8d26faff9bc1fa8fe90295dfbd78bdcab79bc585fe5ce86ab00d69cd64f6802f"
login_salt = "ydsh3JL1nhyY7KMzNxtLCihmYNBQ1wQ0"

result = hashlib.sha3_256((admin_hash + login_salt).encode()).hexdigest()
print(result)
```

Output:

```kotlin
2e7a9d... (send this to the server)
```
Step 4 – Send Computed Hash to Server
Paste it into the prompt, and you get:

```css
Access granted. Flag: saic{s0m3tim3s_7he_h4sh_is_ju57_as_v@lu4bl3_45_7h3_p4ssw0rd!}
```

---

## 🧠 Root Cause
The server relies on a hash chain for authentication (SHA3_256(admin_hash + login_salt)).

But by leaking admin_hash to the client, the security is completely broken.

No need to brute force or reverse — we short-circuit the authentication.

---

## 🛠️ Full Exploit Script

```python
import socket
import hashlib

def compute_hash(admin_hash, login_salt):
    return hashlib.sha3_256((admin_hash + login_salt).encode()).hexdigest()

# Connect to server
s = socket.socket()
s.connect(('iitmandi.co.in', 8096))
print(s.recv(4096).decode())  # Welcome msg

# Step 1: Leak values
s.sendall(b'\x7f\n')
leak = s.recv(4096).decode()
print("[*] Leak:\n", leak)

admin_hash = leak.split("admin: ")[1].split("\n")[0].strip()

# Step 2: Login as admin
s.sendall(b'1\n')
s.recv(1024)  # "username: "
s.sendall(b'admin\n')

# Step 3: Get login_salt and compute hash
prompt = s.recv(1024).decode()
login_salt = prompt.split("'")[1]
print(f"[*] Login salt: {login_salt}")

final_hash = compute_hash(admin_hash, login_salt)
s.sendall((final_hash + '\n').encode())

# Step 4: Receive flag
print(s.recv(4096).decode())
```

---

## ✅ Flag

```css
saic{s0m3tim3s_7he_h4sh_is_ju57_as_v@lu4bl3_45_7h3_p4ssw0rd!}
```
