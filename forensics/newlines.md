# 🔐 Newlines

> **Challenge Name:** Newlines   
> **Category:** Forensics
> **File:** ```newlines.zip```

---

## 📝 Challenge Description

Some secrets hide in plain sight, while others lurk between the lines. Can you find what’s hidden in this seemingly ordinary text file?

---

## 🧠 First Look

We are given 69 txt files. Upon opening them up in hex editor, we can clearly see a pattern. All files are filled with hex 0x0D 0x0A pairs and end with hex 0x23 (#).

---

## 🛠️ Approach

### 1. Confirming the pattern.

First, I wrote a python script to make sure all the files follow this exact pattern. And the result was positive.

### 2. Count the number of pairs
I modified the python script to count the number of 0D 0A pairs in a file, and store them. 

### 3. Mapping to ASCII
I then mapped the number of pairs in a file to the ascii character like `chr(no. of pairs)`. Finally I concactenated all the characters in serial order to get the flag. 

### Final Python Script
```python
import os

folder_path = "newlines"  
x = [' ']*70
def count_0D0A_pairs(file_path, n):
    with open(file_path, "rb") as f:
        data = f.read()

    if data.endswith(b'#'):
        data = data[:-1]
    count = data.count(b'\x0D\x0A')
    x[n] = chr(count)


for filename in os.listdir(folder_path):
    if filename.endswith(".txt"):
        n = int(filename.split(".txt")[0])
        file_path = os.path.join(folder_path, filename)
        count_0D0A_pairs(file_path, n)


print("".join(x))
```

---

🏁 Final Flag
```
saic{s0_m4ny_n3wl1n35_b7w_7h15_15_4_v3ry_l0ng_fl4g_y0u_wr073_5cr1p7!!}
```
