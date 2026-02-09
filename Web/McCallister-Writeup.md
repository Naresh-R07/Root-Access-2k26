---
layout: writeup
title: The McCallister Security Audit
category: Web Security
difficulty: Medium
status: ✅ Completed
---

# 🌐 The McCallister Security Audit

**Category:** Web Security | **Difficulty:** Medium | **Status:** ✅ Completed

---

## 📋 Challenge Overview

This challenge simulated a high-security "smart vault" infiltration. The objective was to locate and combine two hidden flag fragments by bypassing network-level filters, performing forensic analysis on image metadata, and applying multi-layered decryption (XOR and ROT13). The solution required identifying that a space in the provided hint was a "red herring" designed to break the decryption alignment.

---

## 🎯 Challenge Approach

**Goal:** Extract and decrypt multiple flag fragments using metadata analysis and cryptographic techniques.

**Key Techniques:**
- User-Agent Spoofing
- Metadata Extraction (EXIF)
- Base64 Decoding
- Known-Plaintext XOR Analysis
- ROT13 Transformation

---

## 🔍 Step 1: Initial Analysis

The audit began with a target web server that initially returned a **403 Forbidden** error.

### Reconnaissance Steps:

1. **Perimeter Testing**
   - The server filtered access based on identity
   - Bypassing required spoofing the User-Agent to a standard browser string
   
2. **Forensic File Inspection**
   - Two primary files analyzed: `kevin_hallway.jpeg` and `buzz_final.jpeg`
   - Both files contained hidden data in their metadata

3. **Metadata Discovery**
   - Using `exiftool`, a hidden URL parameter was discovered in `kevin_hallway.jpeg`:
   - Parameter: `?page_id=0h_n0_i_f0rg0t`
   - This granted access to a hidden directory containing:
     - `secret_part1.txt`
     - `hidden_data.txt`

---

## 🔐 Step 2: Core Technique - XOR & ROT13 Decryption

### Technique Analysis:

The core technique involved **Known-Plaintext Analysis** and **Repeating XOR Decryption**.

#### Initial Observations:
- Data consisted of two hex strings
- Hint provided in Base64: `Um9hZCBSdW5uZXIg` → "Road Runner"

#### Critical Discovery:
- **First attempt:** XOR with "Road Runner" (with space) → partial success ("root") + gibberish
- **Key insight:** Space was a red herring!
- **Correct key:** `RoadRunner` (no space)
- **Secondary step:** Output required **ROT13** transformation

---

## 💻Step 3: Implementation

### 3.1 Fragment Retrieval

```
Part 1: 370d0303293e5d18541c0d5e123b662a235a
Part 2: 1606611d3e343e41000056000d5d51566708
Full Hex: 370d0303293e5d18541c0d5e123b662a235a1606611d3e343e41000056000d5d51566708
```

### 3.2 XOR Decryption

```python
key = "RoadRunner"
ciphertext = bytes.fromhex("370d0303293e5d18541c0d5e123b662a235a1606611d3e343e41000056000d5d51566708")
xor_result = "".join(chr(ciphertext[i] ^ ord(key[i % len(key)])) for i in range(len(ciphertext)))
```

### 3.3 ROT13 Transformation

```python
import codecs
final_flag = codecs.encode(xor_result, 'rot13')
```

---

## 🏁 Final Flag

```
root{K3v1n_1s_4_M4st3r_Pl4nn3r_2025}
```

---

## 🛠️ Tools Used

| Tool | Purpose |
|------|---------|
| **exiftool** | Extracting metadata from images |
| **Base64 Decoder** | Decoding hints |
| **Python 3** | XOR & ROT13 decryption |
| **curl** | User-Agent spoofing |

---

## 📚 Key Takeaways

1. Metadata is crucial - EXIF data often contains hidden information
2. Context matters - hints can be intentionally misleading  
3. Multi-layer decryption - never assume a single method
4. Known-plaintext attacks help crack XOR keys
5. User-Agent spoofing is an effective WAF bypass

---

<div align="center">

**[← Back to Home](../README.md)** | **[← Previous](../Sanity/Abroad-Writeup.md)** | **[Next →](pdf-web-Writeup.md)**

**Web Security** | ⭐⭐⭐ Medium | ✅ Completed

*Last Updated: February 9, 2026*

</div>


Approach
This challenge simulated a high-security "smart vault" infiltration. The objective was to locate and combine two hidden flag fragments by bypassing network-level filters, performing forensic analysis on image metadata, and applying multi-layered decryption (XOR and ROT13). The solution required identifying that a space in the provided hint was a "red herring" designed to break the decryption alignment.

Step 1: Initial Analysis
The audit began with a target web server that initially returned a 403 Forbidden error.

The first steps included:

Perimeter Testing: Identifying that the server filtered access based on identity. Bypassing this required spoofing the User-Agent to a standard browser string.

Forensic File Inspection: Two primary files, kevin_hallway.jpeg and buzz_final.jpeg, were analyzed for hidden data.

Metadata Discovery: Using exiftool, a hidden URL parameter was discovered in the metadata of kevin_hallway.jpeg: ?page_id=0h_n0_i_f0rg0t.

This parameter granted access to a hidden directory containing two text files: secret_part1.txt and hidden_data.txt.

Step 2: Core Technique
The core technique involved Known-Plaintext Analysis and Repeating XOR Decryption.

The data found in the text files consisted of two hex strings. A hint was provided in Base64: Um9hZCBSdW5uZXIg, which decoded to "Road Runner".

This method was chosen because:

Initial attempts to XOR the strings using "Road Runner" (with a space) resulted in partial successes (the word "root") followed by gibberish.

By calculating the key based on the known flag header root{, it became clear the key was actually RoadRunner (no space).

XOR alone was insufficient; the output required a secondary ROT13 transformation to become readable.

Step 3: Implementation
1. Fragment Retrieval
The hex fragments were extracted and concatenated:

Part 1: 370d0303293e5d18541c0d5e123b662a235a

Part 2: 1606611d3e343e41000056000d5d51566708

Full Hex: 370d0303293e5d18541c0d5e123b662a235a1606611d3e343e41000056000d5d51566708

2. XOR Decryption
A Python script was used to XOR the combined hex against the repeating key RoadRunner.

Python
key = "RoadRunner"
ciphertext = bytes.fromhex("370d0303293e...6708")
xor_res = "".join(chr(ciphertext[i] ^ ord(key[i % len(key)])) for i in range(len(ciphertext)))
3. ROT13 Transformation
The XOR output was passed through a ROT13 shift to finalize the decryption.

Python
final_flag = xor_res.translate(str.maketrans("A-Za-z", "N-ZA-Mn-za-m"))
Step 4: Extraction and Flag Assembly
The decryption process successfully transformed the hex data into the final flag string. The key takeaway was the precision required in the XOR key; the removal of the space character was essential for character alignment across the 36-byte payload.

Final Extracted Data:

Decrypted Body: root{K3v1n_1s_4_M4st3r_Pl4nn3r_2025}

Flag
root{K3v1n_1s_4_M4st3r_Pl4nn3r_2025}

Tools Used
Exiftool – Extracting hidden URL parameters from image metadata.

Base64 Decoder – Decoding the "Road Runner" hint.

Python3 – Implementing the XOR and ROT13 decryption logic.

Curl – Bypassing WAF filters via User-Agent spoofing.