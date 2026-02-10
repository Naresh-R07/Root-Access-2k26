# 🛠️ RootAccess CLI

**Category:** Reverse Engineering

## 🎯 Challenge Approach

**Goal:** We load the modules directly in Node.js and call the exported methods to extract and reassemble the flag.

## 🔍 Solution

- Installed the npm package globally: `npm install -g root-access`.
- Located the source at `npm root -g` → `node_modules/root-access/dist/lib/`.
- Inspected `package.json` — the build script reveals it uses `javascript-obfuscator` with RC4 string encoding.
- The package contains 5 obfuscated modules: `index.js`, `crypto.js`, `keyforge.js`, `network.js`, `vault.js`.
- Key finding: All modules use `module.exports` exposing their classes, meaning we can `require()` them and call methods directly without deobfuscation.
- This approach was chosen because the obfuscation (RC4 string arrays, control flow flattening, dead code injection) makes static analysis extremely time-consuming, but the code is fully functional and exports clean class interfaces.

### Key Observation
- `CryptoEngine` has a `getFragment(n)` method and a `__getFullFlag__()` hidden static method, and `DataVault` has an `assembleFlag()` method — all directly callable.

- Loaded all 5 modules via `require()` in a Node.js script.
- Used `Object.getOwnPropertyNames()` on each class and its prototype to discover all methods.
- Called `CryptoEngine.getTotalFragments()` → returns `10`.
- Called `CryptoEngine.getFragment(i)` for i = 1 to 10 — each fragment is decoded at runtime by: base64-decode → XOR with key `SHADOWGATE`.
- Called `CryptoEngine.__getFullFlag__()` to get the complete flag in one call.

## 🏁 Final Flag

```
root{N0d3_Cl1_R3v3rs3_3ng1n33r1ng_M4st3r_0f_Sh4d0ws}
```

---

<div align="center">

**[← Back to Home](../README.md)** | **[← Previous](RootofTrust-Writeup.md)** | **[Next →](Rootium-Writeup.md)**

**Reverse Engineering** 

*Last Updated: February 9, 2026*

</div>
