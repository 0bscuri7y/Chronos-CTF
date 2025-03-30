# 🔐 RSARSARSA - Crypto CTF Challenge Writeup

> **Challenge Name:** RSARSARSA  
> **Category:** Cryptography  
> **File Provided:** ```chall.py, out.txt```

## 📜 Challenge Description

> Someone really wanted to make sure this message stays hidden!  
> Unfortunately for them, this might have made things easier for you.  
> Can you recover the flag from this overcooked RSA encryption? 🔢🔓

---

## 🔍 Challenge Analysis

The challenge encrypts the **same message** using RSA **three times** with:

- Different moduli `n1`, `n2`, `n3`
- Same public exponent `e = 3`
- No padding

This makes it vulnerable to **Håstad’s Broadcast Attack**, which exploits:

> If the same plaintext `m` is encrypted with the same small exponent `e` under different RSA moduli, and `m^e < n1 * n2 * n3`, we can recover `m` using the **Chinese Remainder Theorem (CRT)** and **integer root extraction**.

---

## 🧠 Exploitation Strategy

### 1. Combine ciphertexts with **CRT**
Reconstruct `m³` modulo `n1 * n2 * n3`.

### 2. Extract the cube root of `m³`
Recover original plaintext `m`.

---

## 🛠️ Decryption Script

```python
from Crypto.Util.number import long_to_bytes, inverse
import gmpy2

# Values from out.txt
e = 3
n1 = <PASTE_FROM_OUT.TXT>
c1 = <PASTE_FROM_OUT.TXT>
n2 = <PASTE_FROM_OUT.TXT>
c2 = c1
n3 = <PASTE_FROM_OUT.TXT>
c3 = c1

# Chinese Remainder Theorem
def crt(c_list, n_list):
    N = 1
    for n in n_list:
        N *= n
    result = 0
    for i in range(len(n_list)):
        ni = n_list[i]
        ci = c_list[i]
        mi = N // ni
        inv_mi = inverse(mi, ni)
        result += ci * mi * inv_mi
    return result % N

# Reconstruct m^3
c_combined = crt([c1, c2, c3], [n1, n2, n3])

# Cube root of m^3 to get m
m = gmpy2.iroot(c_combined, e)[0]

# Decode
flag = long_to_bytes(m)
print(flag.decode())
```

---

## 🏁 Flag

```css
saic{my_7r1pp3ll3d_RSA_D!D_n07_Co0k}
```
