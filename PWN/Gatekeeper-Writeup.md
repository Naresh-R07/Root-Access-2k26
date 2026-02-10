# 🕶️ Gate Keeper

**Category:** PWN (Exploitation)

## 🎯 Challenge Approach

**Goal:** Check through server and find the key.

## 🔍 Solution

- Connected to gatekeeper at `nc gatekeep.rtaccess.app 9999`.
- Ran format string probes (`%p`, `%6$p`, `%7$s`) to map argument offsets and leaks.
- Primary technique: format string write to redirect control flow. Instead of brute-forcing libc (ASLR + many candidates), I dumped .text via `%7$s` reads and found a built-in "win" routine at `0x4012b6` that loads `/bin/sh` and calls `system`.
- Key observation: Uninitialized GOT slots pointed to nearby `.text` addresses (`0x401030`, `0x401040`, `0x401060`). Overwriting any of them with the win address would lead to `system("/bin/sh")` on the next call.
- Crafted a `%hn` payload writing `0x12b6` to `GOT[0x404018]`. The format string `%1$4790c%8$hn`.
- Sent `exit` after the write.
- Verified the shell by sending `id`/`echo`/`cat /flag*` commands.
- The overwritten GOT call launched `/bin/sh`, and `cat /flag*` revealed the flag.

## 🏁 Final Flag

```
root{i_w4n+_t0_br34k_fr33}
```

---

<div align="center">

**[← Back to Home](../README.md)** | **[← Previous](../OSINT/Vigenere-Writeup.md)** | **[Next →](Games-Writeup.md)**

**PWN**

*Last Updated: February 9, 2026*

</div>
