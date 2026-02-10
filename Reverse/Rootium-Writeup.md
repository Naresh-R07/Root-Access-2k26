
# 🛠️ Rootium Browser

**Category:** Reverse Engineering
---

## 📋 Challenge Overview

[Challenge description to be added]

---

## 🎯 Challenge Approach

**Goal:** [Goal statement]

**Key Techniques:**
- Browser binary analysis
- Javascript analysis
- DOM manipulation detection

---

## 🔍 Solution

- **Provided files:** `rootium-browser_1.0.0_amd64.deb` (Linux installer) and `rootium-browser-setup.exe` (Windows installer) — both packaging the same Electron app.
- Extracted the `.deb` archive (ar format) → unpacked `data.tar.xz` → found the Electron app at `opt/rootium-browser/resources/app.asar`.
- Extracted `app.asar` using `@electron/asar`, revealing:
  - `main.js` — Electron main process handling IPC between the UI and the vault binary.
  - `app/index.html` — Browser UI with a vault panel (password input + "Retrieve Root Flag" button).
  - `bin/rootium_vault` — Native ELF x86-64 binary (the authentication backend).
  - `bin/rootium_vault.exe` — Windows PE equivalent.
- **Key finding in `main.js`:** The app spawns `rootium_vault` as a child process and communicates via stdin/stdout using a simple text protocol:
  - Sends `AUTH <password>\n` → expects `ACCESS_GRANTED` or `ACCESS_DENIED`
  - Sends `GET_FLAG\n` → expects `FLAG:<data>`
- **Technique:** Static binary reverse engineering of the `rootium_vault` ELF binary using disassembly (Python + Capstone).
- The binary contains two custom encoding functions:
  1. **Password decode** (function at `0x125c`): Decodes the stored encrypted master password.
  2. **Flag decode** (function at `0x130A`): Decodes the stored encrypted flag using the decoded password as a key.
- Both functions use a combination of **XOR encoding** and **bitwise rotation** (ROR), but with different parameters — ruling out a single generic decoder.
- **Key observation:** The encrypted password and flag are stored in the `.data` section of the ELF binary, and the decryption keys/constants are embedded directly in the code.

#### Password Decode (function `0x125c`)
Extracted encrypted password (22 bytes at file offset `0x3080`) and the 7-byte XOR key `[0xD8, 0xC5, 0xC5, 0xDE, 0xC3, 0xDF, 0xC7]` (which decodes to the ASCII string `"rootium"` when XOR'd with `0xAA`).

Algorithm for each byte `i`:
```
byte = encrypted[i]
byte ^= 0x42
byte = rotate_right(byte, 3)
byte ^= key[i % 7]
```

This yielded the master password: **`v4ult_m4st3r_p4ss_2026`**

#### Flag Decode (function `0x130A`)
Extracted encrypted flag (38 bytes at file offset `0x30A0`). This function takes the **decoded password** as its XOR key.

Algorithm for each byte `i`:
```
byte = encrypted[i]
byte ^= 0x7E
byte = rotate_right(byte, i % 8)
byte ^= password[i % password_length]
```
- **Password extracted:** `v4ult_m4st3r_p4ss_2026` — unlocks the Secret Vault.
- **Flag extracted** by applying the second decode function using the password as the key:
---

## Tools Used  

- **Python 3** – Archive extraction (ar format), disassembly scripting, and decode implementation
- **Capstone** – x86-64 disassembly engine for reversing the native binary
- **@electron/asar** – Extracting the Electron app.asar package
---

## 🏁 Final Flag

```
root{n0_m0r3_34sy_v4ult_3xtr4ct10n_99}
```

---

<div align="center">

**[← Back to Home](../README.md)** | **[← Previous](RootAccessCLI-Writeup.md)** | **[End of Challenges](../README.md)**

**Reverse Engineering**
*Last Updated: February 9, 2026*

</div>
