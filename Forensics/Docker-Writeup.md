---
layout: writeup
title: Docker Mystery Challenge  
category: Forensics
difficulty: Hard
status: ✅ Completed
---

# 🕵️ Docker Mystery Challenge

**Category:** Forensics | **Difficulty:** Hard | **Status:** ✅ Completed

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

**Forensics** | ⭐⭐⭐⭐ Hard | ✅ Completed

*Last Updated: February 9, 2026*

</div>


Approach

This challenge was about analyzing a Docker image that contained a flag split into multiple parts. Each part was hidden using a different Docker feature such as image layers, environment variables, labels, and hidden files. The main goal was to inspect the image carefully, understand how it was built, and recover all the hidden pieces step by step before combining them into the final flag.

Step 1: Initial Analysis

The challenge provided a Docker image hosted on GitHub Container Registry.
The first thing we did was pull the image locally and inspect it.

docker pull ghcr.io/uttam-mahata/rootaccess:latest


After pulling the image, we ran docker inspect and docker history to get an overview of how the image was built and what metadata it contained.

docker inspect ghcr.io/uttam-mahata/rootaccess:latest
docker history ghcr.io/uttam-mahata/rootaccess:latest


From the startup banner inside the container, it was clear that:

The flag was split into four parts

Docker layers and deleted files were important

A fake flag existed to mislead solvers

This confirmed that the challenge was focused on Docker image forensics rather than exploiting the running application.

Step 2: Core Technique

The main technique used was Docker image and layer analysis.

Docker images are built in layers, and even if files are deleted later, their contents can still be recovered from previous layers. The challenge hints strongly suggested checking:

Image metadata (ENV and LABEL)

Deleted files in earlier layers

Hidden files inside the container filesystem

Because of these hints, we focused entirely on inspecting how the image was constructed.

Step 3: Implementation
Extracting FLAG_PART_2 (Environment Variable)

Using docker inspect, we found an environment variable containing part of the flag:

FLAG_PART_2=D0ck3r_


This confirmed that one fragment was intentionally stored in the image metadata.

Extracting FLAG_PART_3 (Image Label)

While inspecting the image labels, another fragment was found:

flag_part_3=1m4g3s_


This showed that labels were also being used to hide flag data.

Extracting FLAG_PART_4 (Hidden File)

To inspect the container filesystem, the entrypoint was overridden so a shell could be accessed:

docker run --rm -it --entrypoint /bin/sh ghcr.io/uttam-mahata/rootaccess:latest


Inside the container, listing the /app directory revealed a hidden file:

ls -la /app


The file .hidden_flag_part4.txt contained the final fragment:

cat /app/.hidden_flag_part4.txt


Output:

N3v3r_F0rg3t}

Extracting FLAG_PART_1 (Deleted Docker Layer)

The most important part was hidden in a deleted layer.
To see the full build commands, Docker history was viewed with full output:

docker history --no-trunc ghcr.io/uttam-mahata/rootaccess:latest


One of the deleted layers contained the following command:

echo "FLAG_PART_1: root{L4y3r3d_" > /tmp/.secret_layer.txt


Although the file itself explains later, the flag part was still visible in the image history.

This revealed FLAG_PART_1.

Step 4: Extraction and Flag Assembly

All extracted parts were:

FLAG_PART_1: root{L4y3r3d_

FLAG_PART_2: D0ck3r_

FLAG_PART_3: 1m4g3s_

FLAG_PART_4: N3v3r_F0rg3t}

These were combined in the order specified by the challenge.

Flag
root{L4y3r3d_D0ck3r_1m4g3s_N3v3r_F0rg3t}

Tools Used

Docker – pulling images, inspecting metadata, analyzing build layers

Linux shell utilities – file and filesystem inspection

Docker history & inspect – recovering deleted data and metadata

Conclusion

This challenge demonstrated how sensitive data can remain inside Docker images even after being deleted. By carefully analyzing image metadata, build history, and container filesystems, all flag fragments were recovered. The challenge emphasized the importance of understanding Docker internals and secure image-building practices.