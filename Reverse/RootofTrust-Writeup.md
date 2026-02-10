
# 🛠️ Root of Trust

**Category:** Reverse Engineering 
---

## 🔍 Solution

- Received a custom ISO (`custom_root_access.iso`) that boots directly into a minimal verification interface with no shell or helpers.
- Extracted the ISO with 7-Zip and located the kernel/initramfs under `boot/`; decompressed `initramfs.cpio.gz` to reveal a lone `init` ELF binary inside.
- Ran `strings`/Python-based extraction against the ELF and spotted prompts like `Enter Authentication Credentials (Flag)` plus the `root{` prefix embedded in `.rodata`.
- The key observation was the validation function reading user input, ROT13-transforming alphabetic characters, XOR-comparing the result to embedded constants, and only accepting a 21-character payload inside `root{...}`.
- Recognizing the ROT13+XOR chain made it possible to reverse the comparison by undoing those operations in code.
- Used Python scripts to read the ELF, map its sections/program headers, and identify the `root{` string at virtual address `0x47925a` and the function that references it (`0x401b86`).
- Disassembled the function to understand the flow: input capture (`read`), ROT13 on letters (function at `0x401725`), length checks (must be 21), and XOR comparisons (`0xAA/0x55`) against hard-coded `movabs` constants.
- Extracted the comparison bytes directly from the immediate values in the assembly, then reversed the XOR mask and ROT13 transform to recover the original payload.
- After reversing the XOR/ROT13 transformations, the payload came out as `Ch41n_0f_Tru5t_Br0k3n`.
- Appending the required prefix/suffix produced the final flag `root{Ch41n_0f_Tru5t_Br0k3n}`.

---

## Tools Used
- 7-Zip – Extracted the ISO and initramfs archives.
- Capstone via Python scripts – Disassembled the statically linked ELF to trace validation logic and constants.
- Python – Automated extraction of strings, ELF metadata inspection, transform inversion, and final flag recovery.
---

## 🏁 Final Flag

```
root{Ch41n_0f_Tru5t_Br0k3n}
```
---

<div align="center">

**[← Back to Home](../README.md)** | **[← Previous](midahfinguh-Writeup.md)** | **[Next →](RootAccessCLI-Writeup.md)**

**Reverse Engineering** 
*Last Updated: February 9, 2026*

</div>
