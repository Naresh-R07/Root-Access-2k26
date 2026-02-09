---
layout: writeup
title: PDF Web Challenge
category: Web Security
difficulty: Medium
status: ✅ Completed
---

# 🌐 PDF Web Challenge

**Category:** Web Security | **Difficulty:** Medium | **Status:** ✅ Completed

---

## 📋 Challenge Overview

A multi-stage challenge combining **source code analysis**, **version control forensics**, and **cryptographic decoding**. The solution involved inspecting page source, discovering a hidden GitHub repository, analyzing commit history for encoded data, and combining multiple decoding techniques to retrieve a remote resource containing the flag.

---

## 🎯 Challenge Approach

**Goal:** Extract flag from obfuscated repository data and remote resources.

**Key Techniques:**
- Source Code Inspection
- Git History Forensics
- Base64 Decoding
- ROT13 Cipher
- Binary String Extraction

---

## 🔍 Step 1: Initial Analysis

### Initial Reconnaissance:

1. **Source Code Inspection**
   - Inspected the HTML source code of the challenge page
   - Discovered a GitHub repository link:
   ```
   https://github.com/Asif-Tanvir-2006/rootaccess_web_chal_pdf/tree/main
   ```

2. **Documentation Review**
   - Read README.md which contained a crucial hint:
   > *"ignore commit histories in your github repos before doing git push --force"*
   
   - This explicitly pointed toward examining **git commit history**

3. **Key Insight:**
   - The challenge instructed to examine what developers typically ignore
   - Suggested critical data was hidden in previous commits

---

## 🔐 Step 2: Core Technique - Multi-Layer Decryption

### Defense in Depth:

The challenge employed **three layers** of obfuscation:

1. **Layer 1:** Git Commit History (encoded data)
2. **Layer 2:** Base64 Encoding
3. **Layer 3:** ROT13 Cipher
4. **Layer 4:** Remote File Storage

### Strategy:

Each technique built upon the previous layer to hide the final flag URL.

---

## 💻 Step 3: Implementation

### 3.1 Repository Cloning

```bash
git clone https://github.com/Asif-Tanvir-2006/rootaccess_web_chal_pdf.git
git log --oneline
```

### 3.2 Commit Analysis

Identified Base64-encoded strings in commit messages:

```
1725e55 dWdnY2Y6Ly9lbmoudHZndWhvaGZyZXBiYWcucGJ6L05mdnMtR25haXZlLTIwMDYvc2JhZ2Yvem52YS9zYmFnLmdncw==
8622249 dWdnY2Y6Ly9lbmoudHZndWhvaGZyZXBiYWcucGJ6L05mdnMtR25haXZlLTIwMDYvc2JhZ2Yvem52YS9zYmFnLmdncw==
```

### 3.3 Base64 Decoding

Using CyberChef or `base64 -d`:

```
Encoded: dWdnY2Y6Ly9lbmoudHZndWhvaGZyZXBiYWcucGJ6L05mdnMtR25haXZlLTIwMDYvc2JhZ2Yvem52YS9zYmFnLmdncw==

Decoded: uggcf://enj.tvguhohfrepbagrag.pbz/Nfvs-Gnaive-2006/sbagf/znva/sbag.ggs
```

### 3.4 ROT13 Transformation

Applying ROT13 cipher:

```
Encoded:  uggcf://enj.tvguhohfrepbagrag.pbz/Nfvs-Gnaive-2006/sbagf/znva/sbag.ggs
Decoded:  https://raw.githubusercontent.com/Asif-Tanvir-2006/fonts/main/font.ttf
```

### 3.5 Remote File Extraction

```bash
# Download font file
wget https://raw.githubusercontent.com/Asif-Tanvir-2006/fonts/main/font.ttf

# Extract strings from binary
strings font.ttf
```

### 3.6 Flag Discovery

The flag was found in the **font file metadata**:

```
root{easy_if_u_follow}
```

---

## 🏁 Final Flag

```
root{easy_if_u_follow}
```

---

## 🛠️ Tools Used

| Tool | Purpose |
|------|---------|
| **git** | Version control & commit history analysis |
| **CyberChef** | Base64 & ROT13 decoding |
| **strings** | Binary string extraction |
| **wget/curl** | Remote file download |

---

## 📚 Key Takeaways

1. **Git history matters** - Always examine `.git` directory for sensitive data
2. **Layered obfuscation** - Complex challenges use multiple encoding techniques
3. **External resources** - Flags can be stored in remote locations
4. **Binary files contain text** - Use `strings` command on executables and fonts
5. **Documentation hints** - README files often contain crucial clues

---

<div align="center">

**[← Back to Home](../README.md)** | **[← Previous](McCallister-Writeup.md)** | **[Next →](../Cryptography/Library-Code-Writeup.md)**

**Web Security** | ⭐⭐⭐ Medium | ✅ Completed

*Last Updated: February 9, 2026*

</div>


---

## Approach  

A multi-stage challenge combining source code analysis, version control forensics, and cryptographic decoding. The solution involved inspecting page source, discovering a hidden GitHub repository, analyzing commit history for encoded data, and combining multiple decoding techniques to retrieve a remote resource containing the flag.

### Step 1: Initial Analysis  
- Inspected the HTML source code of the main challenge page.
- Discovered a GitHub repository link: `https://github.com/Asif-Tanvir-2006/rootaccess_web_chal_pdf/tree/main`
- Read the README.md file which contained a deliberate hint: *"ignore commit histories in your github repos before doing git push --force"*
- The hint explicitly pointed toward examining git commit history as the key to solving the challenge.
- This observation was crucial: the challenge instructed to examine what developers typically ignore (commit history), suggesting critical data was hidden in previous commits.

### Step 2: Core Technique  
- **Git Forensics**: Analyzed commit history to discover encoded data left in commit messages.
- **Base64 Decoding**: Decoded obfuscated URLs hidden in commit messages using Base64 encoding.
- **ROT13 Cipher**: Applied ROT13 decryption to further obscured decoded URLs.
- **Remote Resource Extraction**: Downloaded and analyzed a font file to extract embedded data.
- Why this approach: The challenge layered multiple obfuscation techniques—commit history, Base64, and ROT13—to hide a URL that pointed to a resource containing the flag. Each technique built upon the previous layer.

### Step 3: Implementation  
- Cloned the repository: `git clone https://github.com/Asif-Tanvir-2006/rootaccess_web_chal_pdf.git`
- Examined commit history: `git log --oneline`
- Identified Base64-encoded strings in commit messages:
  ```
  1725e55 dWdnY2Y6Ly9lbmoudHZndWhvaGZyZXBiYWcucGJ6L05mdnMtR25haXZlLTIwMDYvc2JhZ2Yvem52YS9zYmFnLmdncw==
  8622249 dWdnY2Y6Ly9lbmoudHZndWhvaGZyZXBiYWcucGJ6L05mdnMtR25haXZlLTIwMDYvc2JhZ2Yvem52YS9zYmFnLmdncw==
  ```
- Decoded Base64 using CyberChef: `dWdnY2Y6Ly9lbmoudHZndWhvaGZyZXBiYWcucGJ6L05mdnMtR25haXZlLTIwMDYvc2JhZ2Yvem52YS9zYmFnLmdncw==` → `uggcf://enj.tvguhohfrepbagrag.pbz/Nfvs-Gnaive-2006/sbagf/znva/sbag.ggs`
- Applied ROT13 cipher to the decoded result: `uggcf://enj.tvguhohfrepbagrag.pbz/Nfvs-Gnaive-2006/sbagf/znva/sbag.ggs` → `https://raw.githubusercontent.com/Asif-Tanvir-2006/fonts/main/font.ttf`
- Downloaded the font file from the decoded and decrypted URL.

### Step 4: Extraction  
- Extracted strings from the font file: `strings font.ttf`
- The flag was present in the font file metadata: `root{easy_if_u_follow}`
- The multi-layered obfuscation (commit history → Base64 → ROT13 → remote file) was designed to obscure the direct path to the flag, requiring understanding of both cryptographic encoding and git repository structure.

---

## Flag  

```
root{easy_if_u_follow}
```

---

## Tools Used  

- Git – Version control and commit history analysis
- CyberChef – Base64 and ROT13 decoding
- strings – String extraction from binary files