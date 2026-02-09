# 🎨 Style Guide - Root Access 2026 CTF Writeups

> A comprehensive guide to the markdown styling and formatting used across all writeups.

---

## 📋 Markdown Standards

### Headers & Hierarchy

Each writeup follows a consistent header structure:

```markdown
# 🌐 Challenge Name          # Main Title (H1)
**Category:** ... | **Difficulty:** ... | **Status:** ...
## 📋 Challenge Overview     # Section Headers (H2)
### Key Points             # Subsections (H3)
#### Details              # Sub-subsections (H4)
```

---

## 🎨 Design Elements

### Emojis Used

| Element | Emoji | Usage |
|---------|-------|-------|
| Web | 🌐 | Web security challenges |
| Cryptography | 🔐 | Cryptographic challenges |
| Forensics | 🕵️ | Forensic analysis challenges |
| Miscellaneous | 🎞️ | Various challenges |
| OSINT | 📸 | Open source intelligence |
| PWN | 🕶️ | Binary exploitation |
| Reverse Engineering | 🛠️ | Reverse engineering challenges |
| Sanity | 🎈 | Warm-up challenges |
| Overview | 📋 | Challenge summaries |
| Goal | 🎯 | Challenge objectives |
| Analysis | 🔍 | Investigation steps |
| Technique | 🔐 | Core methods |
| Implementation | 💻 | Code/execution steps |
| Flag | 🏁 | Final solution |
| Tools | 🛠️ | Required utilities |
| Takeaways | 📚 | Lessons learned |
| Navigation | 🔗 | Links between files |
| Home | 🏠 | Back to README |
| Next | → | Proceed to next |
| Previous | ← | Go to previous |

---

## 🎯 Formatting Standards

### Code Blocks

Use language-specific syntax highlighting:

```python
# Python code
key = "RoadRunner"
result = xor_decrypt(data, key)
```

```bash
# Bash commands
docker inspect image_name
git log --oneline
```

```markdown
# Markdown example
# This is a header
**Bold text**
*Italic text*
```

### Tables

Standard information tables:

```markdown
| Column 1 | Column 2 | Column 3 |
|----------|----------|----------|
| Data 1   | Data 2   | Data 3   |
| Data 4   | Data 5   | Data 6   |
```

### Lists

- **Bullet Lists:** For unordered information
- **Numbered Lists:** For sequential steps
- **Definition Lists:** For technical terms

---

## 🔗 Navigation Structure

### Standard Navigation Footer

Every writeup ends with:

```markdown
<div align="center">

**[← Back to Home](../README.md)** | **[← Previous](previous-file.md)** | **[Next →](next-file.md)**

**Category** | ⭐ Difficulty | Status

*Last Updated: February 9, 2026*

</div>
```

### Difficulty Ratings

- ⭐ Easy (Sanity)
- ⭐⭐ Medium (Web, Cryptography, OSINT)
- ⭐⭐⭐ Medium-Hard (Some Reverse Engineering)
- ⭐⭐⭐⭐ Hard (PWN, Advanced Reverse Engineering, Forensics)

### Status Indicators

- ✅ Completed
- 🔄 In Progress
- 🔄 Pending
- 🟢 Verified

---

## 📐 Document Structure

### Standard Writeup Template

```markdown
---
layout: writeup
title: Challenge Name
category: Category Type
difficulty: Level
status: Status
---

# [Emoji] Challenge Name

**Category:** ... | **Difficulty:** ... | **Status:** ...

---

## 📋 Challenge Overview
[2-3 paragraph overview]

---

## 🎯 Challenge Approach
**Goal:** [Main objective]
**Key Techniques:** [Bulleted list]

---

## 🔍 Step 1: Initial Analysis
[Analysis details]

---

## 🔐 Step 2: Core Technique
[Technical explanation]

---

## 💻 Step 3: Implementation
[Code and execution details]

---

## 🏁 Final Flag
```
flag_here
```

---

## 🛠️ Tools Used
[Table of tools and purposes]

---

## 📚 Key Takeaways
[Numbered list of lessons]

---

[Navigation footer]
```

---

## 🎨 Visual Hierarchy

### Colors (Markdown compatible)

Since GitHub Markdown has limited styling:
- **Bold** for emphasis: `**text**`
- *Italic* for subtle: `*text*`
- `` `code` `` for technical terms
- `> Blockquotes` for important notes

### Spacing

- Blank lines between major sections
- `---` horizontal rules to separate sections
- Proper indentation within lists

---

## ✅ Checklist for New Writeups

- [ ] Use proper YAML frontmatter
- [ ] Include all required section headers
- [ ] Add appropriate emojis
- [ ] Include challenge overview
- [ ] Explain approach and techniques
- [ ] Provide step-by-step implementation
- [ ] Show final flag in code block
- [ ] List all tools used in table
- [ ] Add key takeaways section
- [ ] Include proper navigation footer
- [ ] Link to related challenges
- [ ] Update README.md with new link
- [ ] Verify all links are correct
- [ ] Test GitHub rendering

---

## 🔄 Directory Structure

```
Root-Access-2k26/
├── README.md (Main index)
├── STYLE_GUIDE.md (This file)
├── Web/
│   ├── McCallister-Writeup.md
│   └── pdf-web-Writeup.md
├── Cryptography/
│   └── Library-Code-Writeup.md
├── Forensics/
│   ├── Docker-Writeup.md
│   ├── Eavesdropping-Writeup.md
│   └── REDACTED-Writeup.md
├── Misc/
│   ├── Dancing-Men-Writeup.md
│   └── Flashes-Writeup.md
├── OSINT/
│   ├── Identity-Crisis-Writeup.md
│   ├── SandBox-Writeup.md
│   └── Vigenere-Writeup.md
├── PWN/
│   ├── Games-Writeup.md
│   └── Gatekeeper-Writeup.md
├── Reverse/
│   ├── midahfinguh-Writeup.md
│   ├── RootofTrust-Writeup.md
│   ├── RootAccessCLI-Writeup.md
│   └── Rootium-Writeup.md
└── Sanity/
    └── Abroad-Writeup.md
```

---

## 📝 Font Recommendations

While Markdown doesn't directly control fonts on GitHub, best practices include:

- **Monospace:** For all code, commands, and technical output
- **Sans-serif:** Default GitHub font (Segoe UI, San Francisco, etc.)
- **Consistent sizing:** Use header hierarchy instead of font sizing

---

## 🔗 Linking Best Practices

### Internal Links

```markdown
[Link Text](relative/path/to/file.md)
[Link with Fragment](file.md#heading-anchor)
```

### External Links

```markdown
[External Resource](https://example.com)
[Tool Documentation](https://docs.tool.io)
```

### Navigation Links

```markdown
**[← Back to Home](../README.md)**
**[Next Challenge →](next-filename.md)**
```

---

## ✨ Tips for Great Writeups

1. **Be Clear:** Explain concepts as if teaching someone else
2. **Show Your Work:** Include intermediate steps and outputs
3. **Add Context:** Explain why techniques work, not just how
4. **Provide Examples:** Use real output from your tools
5. **Keep It Concise:** Remove unnecessary information
6. **Stay Organized:** Follow the standard template
7. **Link Everything:** Reference other writeups where relevant
8. **Update Dates:** Keep the "Last Updated" field current

---

<div align="center">

**[← Back to Home](README.md)**

*Style Guide v1.0 | Last Updated: February 9, 2026*

</div>
