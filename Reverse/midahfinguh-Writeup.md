# 🛠️ mid ahh finguh

**Category:** Reverse Engineering

## 📋 Challenge Overview

This multi-stage steganography and reverse engineering challenge involved:
1. Extracting hidden data from a PNG image.
2. Analyzing an embedded ELF binary.
3. Decrypting a password-protected PDF using visual clues.
4. XOR-decrypting the final flag.

The challenge required combining clues from multiple sources including image analysis, binary reverse engineering, and cryptographic decoding.

## 🎯 Challenge Approach

**Goal:** Extract flag through multi-layer steganography and reverse engineering.

**Key Techniques:**
- PNG LSB Steganography
- ELF Binary Analysis
- Visual Cryptanalysis
- XOR Cipher Decoding
- PDF Password Recovery

## 🔍 Step 1: Initial Analysis

### Provided File:
`middle_fingu.png` - An image of a wrapped finger sculpture (Sukuna character)

### Initial Reconnaissance:

Using `binwalk` and `zsteg`:

- PNG contained extra data after IEND chunk (35,986 bytes at offset `0x6cc7`).
- Base64-encoded message found:
  ```
  WU9VX0NBTl9PTkxZX1VOVE9DS19NRV9XSIVOX0lfQU1fQVRfTVlfRlVMTF9QT1dFUg==
  ```
  Decoded: `YOU_CAN_ONLY_UNLOCK_ME_WHEN_I_AM_AT_MY_FULL_POWER`

- ELF binary embedded at offset 74.
- Encrypted PDF document embedded at offset 23,952.
- Challenge hint: "Nasty aah finger :0" suggesting "0 eyes".

## 🔐 Step 2: Core Technique

### Multi-Layer Decryption:

1. LSB Steganography - Data hidden in PNG extradata.
2. ELF Binary Analysis - Reverse engineering executable behavior.
3. Visual Cryptanalysis - Using image context to derive PDF password.
4. XOR Cipher Decoding - Final flag decryption.

### Key Observations:

- Base64 hint mentioned "FULL_POWER" (incomplete data clue).
- Binary contained: "THE MIDDLE ONE IS THE BEST" (cryptic clue).
- Binary output ROT-cipher encrypted strings based on input.
- Image depicted Sukuna's finger (anime reference - 20 fingers total).
- PDF riddle: "how many eyes? how many ropes"
  - Answer: 0 eyes (from `:0` emoticon), 1 rope (visible finger).
- XOR encryption used key 69 (thematic with "nasty finger").

## 💻 Step 3: Implementation

### 3.1 Extract ELF Binary

```bash
# Find ELF magic bytes in extracted data
grep -abo $'\x7f\x45\x4c\x46' full_extradata.bin
# Output: 69:ELF

# Extract binary from correct offset
dd if=full_extradata.bin bs=1 skip=69 of=correct_binary
chmod +x correct_binary
```

### 3.2 Analyze Binary Behavior

```bash
# Run the binary with different inputs
echo -e "1\n3" | ./correct_binary

# Use ltrace to observe internals
ltrace ./correct_binary
# Found: comparisons against "noh this ain't the string dawg"
# Found: character-by-character cipher generation
```

### 3.3 Extract and Decrypt PDF

```bash
# Extract PDF from binary at discovered offset
dd if=correct_binary bs=1 skip=23952 of=extracted.pdf

# Decrypt using context password (Sukuna has 20 fingers)
qpdf --password=20 --decrypt extracted.pdf unlocked.pdf
```

### 3.4 Analyze PDF Content

- Cipher string: `7**1>//.7v#v7v+&vpq7v#0+8`
- Riddle answer: 0 eyes, 1 rope.
- Analyzed PDF structure for character font mappings.

### 3.5 XOR Cipher Decoding

```python
cipher = b"7**1>//.7v#v7v+&vpq7v#0+8"

for k in range(1, 256):
    out = bytes(c ^ k for c in cipher)
    if all(32 <= b <= 126 for b in out):
        print(k, out.decode())

# Key 69: root{jjkr3f3r3nc354r3fun}
```

## 🏁 Final Flag

```
root{jjkr3f3r3nc354r3fun}
```

## 🛠️ Tools Used

| Tool | Purpose |
|------|---------|
| zsteg | PNG steganography detection |
| binwalk | File analysis & embedded data |
| dd | Binary extraction at offsets |
| file | File type identification |
| ltrace | Library call tracing |
| qpdf | PDF decryption and analysis |
| grep | Pattern matching |
| xxd | Hexadecimal dump analysis |
| Python 3 | XOR cipher decoding |

## 📚 Key Takeaways

1. Multi-layer obfuscation - Data hidden across multiple formats.
2. Context is crucial - Anime/meme references provide password hints.
3. Binary analysis required - Reverse engineering reveals encryption methods.
4. Visual clues matter - Image analysis provided PDF password.
5. Steganography common - PNG metadata often contains hidden data.
6. Thematic consistency - Key numbers relate to challenge theme.

---

<div align="center">

**[← Back to Home](../README.md)** | **[← Previous](../PWN/Gatekeeper-Writeup.md)** | **[Next →](RootofTrust-Writeup.md)**

**Reverse Engineering**

*Last Updated: February 9, 2026*

</div>
