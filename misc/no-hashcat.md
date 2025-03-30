
## 🧩 Challenge Description
We were given a single Yescrypt hash and asked to find the original password/flag. The hash format was:

```swift
$y$j9T$s/UaF7EiN0ylLVryw75Co1$6WdEcyhcU.f8Fa/HhKBHCPC7qUN3fSxbNR3izAHNvE6
```

The hint mentioned:

- The hash contains parts of the flag.
- The flag format follows saic{...}.

---

## 🔍 Approach

Initially, we attempted to identify the hash via Hashes.com. It confirmed the format as Yescrypt, noted by the $y$ prefix.

![image](https://github.com/user-attachments/assets/856343d4-308c-43ae-ac15-e94418f0e1f0)

Since Hashcat doesn’t support this format, we pivoted to using John the Ripper, which supports cracking Yescrypt hashes under the --format=crypt option.

We then first copy this hash text to hash.txt

---

## 🛠️ Solution Strategy
The password embedded in the hash follows a CTF flag format: saic{...}. Therefore, brute-forcing with a standard wordlist wouldn’t be ideal.

To maximize efficiency:

We generated a modified wordlist where each line wraps common words with the saic{} flag format.

We then used this list with John the Ripper to attempt a match.

---

## 🧪 Steps

1. Prepare the Wordlist
```python
with open('/usr/share/wordlists/rockyou.txt', 'r', encoding="latin-1") as f:
    lines = f.readlines()

with open('modified_wordlist.txt', 'w') as f:
    for line in lines:
        f.write('saic{' + line.strip() + '}\n')
```

2. Crack the Hash with John
```bash
john --format=crypt --wordlist=modified_wordlist.txt hash.txt
```

```bash
--format=crypt: Required for Yescrypt hashes.

```
```bash
--wordlist: Uses the custom list we generated.
```
hash.txt: File containing the original Yescrypt hash.

---

## 🎯 Final Flag

```css
saic{backstreetboys}
```
