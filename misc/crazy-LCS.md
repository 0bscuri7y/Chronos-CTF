# 🔥 crazy-LCS — DP Longest Common Substring Challenge

> **Challenge Name:** crazy-LCS  
> **Category:** Misc / Reverse Engineering / Algorithms  
> **Connection:** `nc iitmandi.co.in 8098`  

---

## 🧩 Challenge Description

> The server plays hot-and-cold with your guesses—only telling you how much contiguous chunk matches the flag. No hints, no mercy, just pain. Can you piece it together, or will you substring yourself into madness? 😈🔍
> You are allowed to interact with the server using a netcat connection. For each guess, it responds with:

```text
LCS length: <n>
This is the length of the Longest Common Substring (LCS) between your guess and the secret flag.
```

---

## 🧠 Understanding the Challenge

> The flag has a fixed format (like saic{...}).
> You can guess the flag one character at a time.
> If your guess is a prefix of the actual flag, the LCS will equal the length of your guess.
> If your guess deviates, the LCS will be shorter.
> You win when your guess exactly matches the flag.

---

## 🚀 Strategy to Solve

We use the property that LCS length = current guess length only when the guess is a correct prefix.

### Steps:
- Start from a known prefix (saic{) which gives LCS = 5.
- Try all printable characters at the next position (saic{a}, saic{b}, ..., saic{}).
- If LCS == previous_length + 1, that character is correct.
- Repeat until the flag ends with }.

---

## ⚙️ Exploit Script

```python
import socket
import string
import concurrent.futures

# Target server details
HOST = 'iitmandi.co.in'
PORT = 8098

# Valid characters to try — includes lowercase, uppercase, digits, and punctuation
CHARS = string.printable.strip()  # or define your own charset for speed

def get_lcs_length(guess):
    try:
        with socket.create_connection((HOST, PORT), timeout=3) as s:
            s.sendall(guess.encode() + b'\n')
            data = s.recv(1024).decode()
            if "LCS length:" in data:
                return int(data.split("LCS length:")[1].strip())
    except Exception as e:
        return -1

# Attempt to find the next character
def try_char(prefix, c, expected_len):
    guess = prefix + c
    lcs = get_lcs_length(guess)
    if lcs == expected_len:
        return c
    return None

def brute_force_flag():
    flag = "saic{"  # Known prefix
    print("[*] Starting brute-force...")
    
    while not flag.endswith("}"):
        found = False
        expected_len = len(flag) + 1
        with concurrent.futures.ThreadPoolExecutor(max_workers=16) as executor:
            futures = {executor.submit(try_char, flag, c, expected_len): c for c in CHARS}
            for future in concurrent.futures.as_completed(futures):
                result = future.result()
                if result:
                    flag += result
                    print(f"[+] Found: {flag}")
                    found = True
                    break
        if not found:
            print("[-] Failed to find next character. Retrying...")
    
    print(f"[✅] Final Flag: {flag}")

if __name__ == '__main__':
    brute_force_flag()
```

This script uses multithreading to accelerate the brute-force of each character, opening a new connection for every guess, and immediately reacting when a correct character is found.

---

## 🏁 Flag

```css
saic{3asy_dp_lc5_ch4ll3ng3}
```
