# 🔐 Catch Me If You Can

> **Challenge Name:** Catch Me If You Can
> **Category:** Reverse  
> **File:** ```clip.exe```

---

## 📝 Challenge Description

This EXE lets you copy something… but before you can paste it, poof—it’s gone! Can you outsmart it?

---

## 🧠 Initial Thoughts

Based on the file size and the icon of the app when runnning (the feather icon), its evident its made with python's tkinter library. 

---

## 🛠️ Steps to Extract & Analyze

### 1. Extract the Contents

I use this script to extract the contents of the exe file - [pyinstxtractor](https://github.com/extremecoders-re/pyinstxtractor)


```bash
python pyinstxtractor.py clip.exe
```

### 2. Decompiling .pyc file
The folder contains a lot of pyc or python bytecode compiled files. I find there exists a clip.pyc file, which appears to be the core file based on the exe name. I used an online decompiler on clip.pyc.

[PyLingual](https://pylingual.io)


### 3. Analyzing decompiled clip.pyc
Upon reading the decompiled code, we can easily see the flag is present in base64 format. We decode it to gt the final flag.


🏁 Final Flag
```
saic{y0u_f!n@lly_c0p!3d_!t,right?}
```
