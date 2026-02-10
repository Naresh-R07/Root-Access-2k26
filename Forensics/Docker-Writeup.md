
# 🕵️ Docker Mystery Challenge

**Category:** Forensics 
---

## 📋 Challenge Overview

This challenge was about analyzing a **Docker image** that contained a flag split into multiple parts. Each part was hidden using different Docker features:
- Image layers
- Environment variables  
- Labels
- Hidden files

The goal was to inspect the image carefully, understand its build process, and recover all flag pieces.

---

## 🎯 Challenge Approach

**Goal:** Extract hidden flag fragments from Docker image metadata and filesystem layers.

**Key Techniques:**
- Docker image inspection
- Layer analysis
- Metadata extraction
- Filesystem forensics

---

## 🔍 Step 1: Initial Analysis

### Challenge Setup:

The challenge provided a Docker image hosted on GitHub Container Registry:

```bash
docker pull ghcr.io/uttam-mahata/rootaccess:latest
```

### Reconnaissance:

After pulling the image, key commands were used:

```bash
docker inspect ghcr.io/uttam-mahata/rootaccess:latest
docker history ghcr.io/uttam-mahata/rootaccess:latest
```

### Key Findings:

- Flag was split into **4 parts**
- Deleted files could be recovered from previous layers
- A fake flag existed to mislead solvers
- Forensics focused on Docker image construction

---

## 🔐 Step 2: Core Technique - Layer Analysis

Docker images are built in layers, and even deleted files remain accessible in earlier layers. The challenge required inspecting:

1. **Image Metadata** (ENV & LABEL)
2. **Deleted files** in earlier layers
3. **Hidden files** inside the container filesystem

---

## 💻 Step 3: Implementation

### 3.1 Extracting FLAG_PART_2 (Environment Variable)

Using `docker inspect`, an environment variable was found:

```
FLAG_PART_2=D0ck3r_
```

**Finding:** One fragment was intentionally stored in image metadata.

### 3.2 Extracting FLAG_PART_3 (Image Label)

Inspecting image labels revealed another fragment:

```
flag_part_3=1m4g3s_
```

**Finding:** Labels also contained hidden flag data.

### 3.3 Extracting FLAG_PART_4 (Hidden File)

To access the container filesystem:

```bash
docker run --rm -it --entrypoint /bin/sh ghcr.io/uttam-mahata/rootaccess:latest
```

Inside the container:

```bash
ls -la /app
cat /app/.hidden_flag_part4.txt
```

**Output:**
```
N3v3r_F0rg3t}
```

**Finding:** Hidden files with dot-prefix concealed flag fragments.

### 3.4 Extracting FLAG_PART_1 (Deleted Docker Layer)

The most critical part was hidden in a deleted layer:

```bash
docker history --no-trunc ghcr.io/uttam-mahata/rootaccess:latest
```

A deleted layer contained:

```
echo "FLAG_PART_1: root{L4y3r3d_" > /tmp/.secret_layer.txt
```

**Finding:** Layer history preserves commands even after files are deleted!

---

## 🏁 Assembling the Flag

```
FLAG_PART_1: root{L4y3r3d_
FLAG_PART_2: D0ck3r_
FLAG_PART_3: 1m4g3s_
FLAG_PART_4: N3v3r_F0rg3t}

Full Flag: root{L4y3r3d_D0ck3r_1m4g3s_N3v3r_F0rg3t}
```

---

## 🏁 Final Flag

```
root{L4y3r3d_D0ck3r_1m4g3s_N3v3r_F0rg3t}
```

---

## 🛠️ Tools Used

| Tool | Purpose |
|------|---------|
| **docker inspect** | View image metadata & ENV variables |
| **docker history** | Inspect build layers and deleted commands |
| **docker run** | Execute container with custom entrypoint |
| **ls -la** | List hidden files (dotfiles) |

---

## 📚 Key Takeaways

1. **Docker layers are permanent** - Deleted files remain in image history
2. **Metadata matters** - ENV variables and labels hide data
3. **Hidden files are common** - Dotfiles are easily overlooked
4. **Layer inspection is crucial** - `docker history --no-trunc` shows everything
5. **Defense recommendation** - Don't store secrets in Docker images (use secrets management)

---

<div align="center">

**[← Back to Home](../README.md)** | **[← Previous](../Cryptography/Library-Code-Writeup.md)** | **[Next →](Eavesdropping-Writeup.md)**

**Forensics**

*Last Updated: February 9, 2026*

</div>
