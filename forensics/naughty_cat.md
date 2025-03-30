# Writeup: Hidden File Extraction from Naughty_Cat.jpg

## Challenge Overview
In this challenge, we encountered a JPEG file named `naughty_cat.jpg` that seemed to hide something suspicious. Our goal was to extract any hidden data embedded within the image using forensic techniques.

---

## Step-by-Step Solution

### 1. **Initial Analysis with Binwalk**
We began by analyzing the `naughty_cat.jpg` file with **Binwalk**, a tool used to detect and extract hidden files and data from images, binaries, and archives.

**Command:**
```bash
binwalk naughty_cat.jpg
```

**Output:**
```
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.01
30            0x1E            TIFF image data, big-endian, offset of first image directory: 8
127888        0x1F390         JPEG image data, JFIF standard 1.0
```

### 2. **Interpreting the Results**
- The output revealed multiple entries, including a **TIFF image** and a **hidden JPEG file** at offset **0x1F390** (decimal value: **127888**).
- This hidden JPEG was likely the embedded content we needed to extract.

---

### 3. **Manual Extraction of the Hidden JPEG**
Using the `dd` command, we manually extracted the hidden JPEG file at the detected offset (127888).

**Command:**
```bash
dd if=naughty_cat.jpg bs=1 skip=127888 of=extracted.jpg
```

**Explanation:**
- `if=naughty_cat.jpg`: Specifies the input file.
- `bs=1`: Reads the file byte by byte.
- `skip=127888`: Skips the first 127,888 bytes to reach the hidden data.
- `of=extracted.jpg`: Writes the extracted hidden data to a new file named `extracted.jpg`.

---

### 4. **Viewing the Extracted File**
After successfully extracting the hidden file, we opened it to inspect the content.

**Command:**
```bash
xdg-open extracted.jpg
```

Upon viewing the image, we found the hidden content, which revealed the flag or clue required for this challenge!

---

## Conclusion
By using Binwalk and the `dd` command, we successfully uncovered the hidden data embedded in `naughty_cat.jpg`. This challenge demonstrated the importance of understanding file formats and offsets when dealing with steganography and hidden data.

---

## Tools Used
- **Binwalk**: For analyzing and detecting hidden files.
- **dd Command**: For manual extraction of data at a specific offset.
- **xdg-open**: To view the extracted file.

---

## Commands Summary
```bash
# Step 1: Analyze the file with Binwalk
binwalk naughty_cat.jpg

# Step 2: Extract the hidden JPEG at offset 127888
dd if=naughty_cat.jpg bs=1 skip=127888 of=extracted.jpg

# Step 3: View the extracted file
xdg-open extracted.jpg
```

---

This writeup showcases the process of uncovering hidden data in a file using forensic techniques and will serve as a helpful guide for similar challenges in the future.

![Hash Identification Screenshot](images/naughty_cat_flag.webp)

