---
layout: writeup
title: The Library Code
category: Cryptography

---

# 🔐 The Library Code

**Category:** Cryptography 

---

## 📋 Challenge Overview

This challenge combines **cryptanalysis** with a hidden encoding hint. The encrypted message must be decrypted using a **Caesar cipher**, but the shift key is cleverly embedded within the challenge description itself.

---

## 🎯 Challenge Approach

**Goal:** Derive the Caesar cipher key from contextual clues and decrypt the message.

**Key Techniques:**
- IP-based Key Derivation
- Caesar Cipher Decryption
- Contextual Analysis

---

## 🔍 Step 1: Initial Analysis

An encrypted message was provided in `encrypted_note.txt`:

```
Dro pvkq sc ss3cd5_vslbkbi
```

### Key Observations:

1. **Challenge Context**
   - Mentioned: "IIEST Library Question Bank"
   - IP Address: `10.11.1.6`

2. **Hint Analysis**
   - "Sum all the essence within"
   - Suggested aggregating numerical values

3. **Ciphertext Properties**
   - Contains both letters and numbers
   - Numbers remained unchanged (substitution only affects letters)

---

## 🔐 Step 2: Key Derivation

### The Hidden Key:

The IP address was identified as the key source: `10.11.1.6`

#### Deriving the Shift Value:

Following the hint "sum all the essence within":

```
1 + 0 + 1 + 1 + 1 + 6 = 10
```

**Shift Value:** `10` (Caesar cipher left shift by 10 positions)

---

## 💻 Step 3: Implementation

### Caesar Cipher Decryption (Shift = 10)

```python
def caesar_decrypt(ciphertext, shift):
    result = ""
    for char in ciphertext:
        if char.isalpha():
            # Shift within alphabet (26 letters)
            base = ord('A') if char.isupper() else ord('a')
            shifted = (ord(char) - base - shift) % 26
            result += chr(base + shifted)
        else:
            # Keep non-alphabetic characters unchanged
            result += char
    return result

ciphertext = "Dro pvkq sc ss3cd5_vslbkbi"
plaintext = caesar_decrypt(ciphertext, 10)
print(plaintext)  # Output: The flag is jj3tul5_hlrbayr
```

### Conversion to Flag Format:

```
The flag is jj3tul5_hlrbayr
→ root{jj3tul5_hlrbayr}
```

---

## 🏁 Final Flag

```
root{jj3tul5_hlrbayr}
```

---

## 🛠️ Tools Used

| Tool | Purpose |
|------|---------|
| **Python 3** | Caesar cipher implementation |
| **Text Analysis** | Pattern recognition |
| **IP Parser** | Extracting numerical clues |

---

## 📚 Key Takeaways

1. **Context clues matter** - Challenge descriptions often embed important information
2. **IP addresses can encode data** - Consider all numerical values as potential keys
3. **Simple ciphers are common** - Caesar cipher remains a CTF staple
4. **Preserve non-alphabetic characters** - Numbers and symbols stay unchanged
5. **Systematic approach** - Test all possibilities methodically

---

<div align="center">

**[← Back to Home](../README.md)** | **[← Previous](../Web/McCallister-Writeup.md)** | **[Next →](../Forensics/Docker-Writeup.md)**

**Cryptography** |
*Last Updated: February 9, 2026*

</div>
