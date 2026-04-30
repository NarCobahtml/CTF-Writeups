# ambalabu

**Category:** Forensics
**Points:** 50
**Author:** Kunyux92

---

## Description

> misteri dibalik kata ambalabu 😱

---

## Official Hints

<details>
<summary>Click to show hints</summary>

- No hints available

</details>

---

## Initial Analysis

The challenge provides an image file `boneka.jpg`.

Observations:

- The description mentions **"ambalabu"**
- Using `exiftool`, we find:
  - Image Description: _waspada sosok amba labu_
  - Artist: _RUSDI&AMBA_

This suggests that the keyword **"ambalabu"** is important.

---

## Recon / Enumeration

Check metadata:

```bash
exiftool boneka.jpg
```

Relevant output:

```text
Image Description : waspada sosok amba labu
Artist            : RUSDI&AMBA
```

---

When copying the strange "ambalabu" text from the challenge into the terminal, it produces unusual invisible characters:

```bash
a<2063><200c><200d>... (truncated)
```

This indicates the presence of **zero-width characters**.

---

## Exploitation

### Step 1 - Identify the Technique

The strange characters suggest **Zero Width Steganography**, where hidden data is encoded using invisible Unicode characters.

---

### Step 2 - Extract Hidden Data

Use an online tool like **StegZero** to analyze the text.

[detect.png]

The tool confirms that hidden data exists in the text.

---

### Step 3 - Decode the Data

Use the key based on metadata:

```text
RUSDI&AMBA
```

Decoding reveals the flag.

[dcode.png]

---

## Flag

```text
REDLIMIT{REDACTED}
```

---

## Notes / Lessons Learned

- Zero-width characters can hide data in plain sight
- Copy-pasting strange text into terminal can reveal hidden Unicode
- Metadata (like Artist) can be used as a decoding key
- Tools like StegZero help detect invisible steganography
