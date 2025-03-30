# 🔐 Tiny, But Secure?

> **Challenge Name:** Tiny, But Secure?    
> **Category:** Reverse  
> **Challenge Type:** Rev   
> **File:** ```core.iso```

---

## 📝 Challenge Description

> My friend said this Tiny Core ISO isn’t secure… but if that’s true, why does it have a password?  
> Maybe there’s some secret deep inside. See if you can find something in the roots.

We're given a file: `core.iso`  
Our goal: analyze this ISO image and uncover any hidden secrets or flags.

---

## 🧠 Initial Thoughts

At first glance, this appears to be a challenge related to forensic ISO inspection or basic reverse engineering. Tiny Core Linux is known for being minimalist, but security isn't always about size. Since the hint suggests something may be “in the roots,” we suspect there’s something buried deep in the file system or boot image.

---

## 🛠️ Steps to Extract & Analyze

### 1. Extract the ISO

We use `7z` (7-Zip) to unpack the ISO contents into a directory:

```bash
7z x core.iso -ocore_iso_contents
cd core_iso_contents
```
### 2. Locate the Boot Image
Navigate into the boot directory. Inside, we find a compressed core.gz — this likely contains the actual root filesystem:

```bash
cd boot
gunzip core.gz
```
Now we have the uncompressed core file.

### 3. Inspect the Contents Using strings
We use strings to extract readable ASCII strings from the binary file. Sometimes flags or secrets are hidden in plain sight like this:

```bash
strings core | less
```

### 4. Discover the Flag 🎯
Scrolling through the output, we find the following flag: saic{you_found_the_core}

---

🏁 Final Flag
```css
saic{you_found_the_core}
```
