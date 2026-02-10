---
layout: writeup
title: Games
category: PWN

---

# 🕶️ Games

**Category:** PWN (Exploitation) 
---

## 🎯 Challenge Approach

**Goal:** Check through Server and find the key
---

## 🔍 Solution

# Challenge Name  
A Game of Circles and Crosses

---
## Solution 
- Provided file: `bugs.py` – A Python Tkinter Tic-Tac-Toe game
- Ran static analysis on the code to understand game logic
- Key findings:
  - Game sends move proofs to `https://rootaccess.pythonanywhere.com/verify`
  - Board uses mixed data types (string `"3"` initial, integer `0` for player, `1` for computer)
  - Multiple bugs in win detection and cell occupancy validation
  - Moves are recorded as `{"x": column, "y": row}` coordinates
- Analyzed how the game constructs the proof payload: - given by chatgpt
  ```python
  p = {"v": 1, "nonce": random_hex, "human": moves_array}
  proof = base64_urlsafe_encode(json.dumps(p)) 
  ```
- Crafted a winning move sequence (3 moves in a line)
- Sent POST request directly to verification endpoint with the proof
- Submitted winning moves for first column: `(0,0), (0,1), (0,2)`
- Server responded with `{"ok": true, "result": "player", "flag": "..."}`
- Multiple winning patterns work: rows, columns, and diagonals all return the flag

## Tools Used  

- Python 3 – Script to craft and send verification payload 
---

## 🏁 Final Flag

```
root{M@yb3_4he_r3@!_tr3@5ur3_w@$_th3_bug$_w3_m@d3_@l0ng_4h3_w@y}
```

---

<div align="center">

**[← Back to Home](../README.md)** | **[← Previous](../OSINT/Vigenere-Writeup.md)** | **[Next →](Gatekeeper-Writeup.md)**

**PWN** 

*Last Updated: February 9, 2026*

</div>