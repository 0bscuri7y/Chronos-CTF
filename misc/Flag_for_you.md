# 🏁 Challenge Writeup: Flag for you

> **Category:** Forensics  
> **Challenge Type:** Misc / Steganography (Text-Based)  
> **File:** flag.txt  

## 🧩 Challenge Description
"Pssshhhh you wanted a flag, Here it is!"

> We were given a single file: flag.txt, which contained a large block of ASCII art composed mostly of symbols like @, %, #, -, etc.

---

## 🔍 Approach
> At first glance, the file looked like standard ASCII art with no readable flag. A quick string search for the saic{ format yielded no results. However, a closer look revealed that certain alphanumeric characters and underscores were embedded sparsely throughout the artwork.

---

## 🛠️ Solution Strategy
The key insight:

The flag is constructed by removing all characters except: A-Z, a-z, 0-9, _, {, and }.

This simple filter exposes the hidden message.

---

## 🧪 Steps

Read the file.
Filter characters, keeping only:
- Uppercase letters
- Lowercase letters
- Digits (0–9)
- Underscore (_)
- Curly braces ({, })

Concatenate the result.

---

### 🧾 Python One-Liner

```python
with open("flag.txt") as f:
    print(''.join(c for c in f.read() if c.isalnum() or c in "_{}"))
```

---

🎯 Output

```css
saic{S4IC_1S_4W3S0M3}
````
