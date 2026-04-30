# Sterafi

**Category:** Forensics
**Points:** 50
**Author:** zfernm

---

## Description

> admin tidak sengaja klik link phising dan muncul gambar tersebut, apakah kamu tau inituh teknik apa?

---

## Official Hints

<details>
<summary>Click to show hints</summary>

- No hints available

</details>

---

## Initial Analysis

The challenge provides a JPEG image file named `Redlimit.jpeg`.

Initial observations:

- The file is an image, but likely contains hidden data
- The description hints at a **technique**, suggesting steganography
- No obvious flag is visible from just opening the image

This indicates that the flag is probably hidden inside the file rather than visible.

---

## Recon / Enumeration

Check the file type:

```bash
file Redlimit.jpeg
```

Output:

```text
Redlimit.jpeg: JPEG image data, JFIF standard 1.01 ...
```

---

Check metadata:

```bash
exiftool Redlimit.jpeg
```

Result:

- No suspicious metadata found

---

Check for embedded files:

```bash
binwalk Redlimit.jpeg
```

Result:

- Only standard JPEG structure

---

Inspect raw bytes:

```bash
xxd Redlimit.jpeg
```

Result:

- Looks like a normal JPEG file

---

At this point, nothing obvious is found, so we move to steganography tools.

---

## Exploitation

### Step 1 - Identify the Technique

Since the file is an image and no metadata or embedded data is visible, a common technique is **steganography**.

One well-known tool for this is `steghide`.

---

### Step 2 - Attempt Extraction

Try extracting hidden data:

```bash
steghide extract -sf Redlimit.jpeg
```

First attempt (empty passphrase):

```text
could not extract any data
```

---

Try a common password such as `admin`:

```bash
steghide extract -sf Redlimit.jpeg
```

Input:

```text
admin
```

Output:

```text
wrote extracted data to "flag.txt".
```

---

### Step 3 - Get the Flag

Read the extracted file:

```bash
cat flag.txt
```

Output:

```text
REDLIMIT{REDACTED}
```

---

## Flag

```text
REDLIMIT{REDACTED}
```

---

## Notes / Lessons Learned

- Steganography is a common technique in forensic challenges
- If metadata and binwalk show nothing, try stego tools
- `steghide` is useful for extracting hidden data from images
- Weak or common passwords (like `admin`) are often used in CTF challenges
- Always try simple passphrases when brute forcing manually
