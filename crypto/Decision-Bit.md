# 🧠 Crypto Writeup – Decision-Bit

> **Challenge Name**: Decision-Bit  
> **Category**: Cryptography  
> **File provided**: ```chall.py, output.txt```  

## 📜 Challenge Description

> Someone tried to be clever and hid the flag… by randomly throwing away some of it!   
> Now all that’s left are some numbers and a bunch of "X"s.   
> Can you put the pieces back together and recover what they tried to erase?   

We are provided two files:   
- chall.py (challenge script)   
- output.txt (obfuscated flag data) 

---

## 🔍 Initial Analysis

Understanding chall.py:
```python
flag_bin = bin(bytes_to_long(flag))[2:].zfill(len(flag) * 8)
```
• The flag is read and converted to a bitstring.

Then for each bit:
```python
if c == "0":
    output.append(random.getrandbits(512) % p)
else:
    output.append("X")
```
What This Means:   
- "0" bits are replaced with a random number mod p   
- "1" bits are represented by the string "X"   
    
So we can reverse engineer the flag by:    
- Replacing each number with '0'   
- Replacing each "X" with '1'   
- Reconstructing the bitstring → integer → bytes → flag   

---

## 🛠️ Exploit Script

```python

from Crypto.Util.number import long_to_bytes

with open("output.txt", "r") as f:
    lines = f.read().strip().splitlines()

bitstring = ''.join(['0' if line != 'X' else '1' for line in lines])

flag = long_to_bytes(int(bitstring, 2))
print(flag.decode())
```

---

## 🎯 Recovered Flag

```css
saic{ch3ck_t0rsion_aft3r_pa1ring}
```
