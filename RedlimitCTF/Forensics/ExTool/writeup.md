# ExTool

**Category:** Forensics
**Points:** 50
**Author:** zfernm

---

## Description

> Apakah kamu tau apa itu metadata? Gimana cara melihatnya?
>
> 📁 Downloads
>
> Download Challenge Files

---

## Official Hints

<details>
<summary>Click to show hints</summary>

- No hints available

</details>

---

## Initial Analysis

The challenge provides a JPEG file named `Redlimit.jpeg`.

Key observations:

- The prompt mentions **metadata**, suggesting hidden information inside the file
- JPEG files often contain metadata such as EXIF or comments
- The flag is likely hidden within this metadata rather than the visible image

---

## Recon / Enumeration

Check the file type:

```bash
file Redlimit.jpeg
```

Output:

```text
Redlimit.jpeg: JPEG image data ... comment: "UkVETElNSVR7RVhJRlRPT0x9"
```

Observation:

- There is a **comment field** containing a Base64-like string

---

Extract metadata using `exiftool`:

```bash
exiftool Redlimit.jpeg
```

Relevant output:

```text
Comment : UkVETElNSVR7RVhJRlRPT0x9
```

---

## Exploitation

### Step 1 - Identify the Encoding

The string:

```text
UkVETElNSVR7RVhJRlRPT0x9
```

resembles Base64 encoding.

---

### Step 2 - Decode the Data

Decode using Base64:

```bash
base64 -d <<< UkVETElNSVR7RVhJRlRPT0x9
```

---

### Step 3 - Get the Flag

The decoded output is:

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

- Metadata can contain hidden information in forensic challenges
- `exiftool` is a powerful tool for inspecting image metadata
- Base64 is commonly used to obfuscate flags
- Always check file comments and metadata when analyzing media files
