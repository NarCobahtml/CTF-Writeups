# Riddle_Regitstry

**Category:** Forensic
**Points:** Unknown

---

## Description

> This challenge provides a single artifact: a PDF file named `confidential.pdf`. The flag is not visible in the document content, so the correct approach is to inspect the file metadata and internal structure.

---

## Official Hints

<details>
<summary>Click to show hints</summary>

* Hint 1: Inspect the file metadata and internal structure

</details>

---

## Initial Analysis

This challenge provides a single artifact: a PDF file named `confidential.pdf`. The flag is not visible in the document content, so the correct approach is to inspect the file metadata and internal structure.

---

## Recon / Enumeration

First, check the files available in the challenge directory:

```bash
ls -la
rg --files
```

The result shows only one main file:

```text
confidential.pdf
```

PDF metadata analysis:

```bash
pdfinfo confidential.pdf
```

Relevant output:

```text
Author: cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9jOTk5ZTJhNH0=
Producer: PyPDF2
```

The value in the `Author` field clearly looks like a Base64-encoded string.

For additional verification, the metadata can also be checked with:

```bash
exiftool confidential.pdf
```

This confirms that the `Author` field contains the same suspicious string.

---

## Exploitation

### Step 1 - Find the Vulnerability

The visible text in the PDF can also be extracted with:

```bash
pdftotext confidential.pdf -
```

However, the extracted text only contains distraction and hints suggesting that the answer is not in the main body of the document. This reinforces the conclusion that the flag is hidden in the metadata.

### Step 2 - Develop the Exploit

The next step is to decode the Base64 value:

```bash
printf '%s' 'cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9jOTk5ZTJhNH0=' | base64 -d
```

Output:

```text
picoCTF{puzzl3d_m3tadata_f0und!_c999e2a4}
```

### Step 3 - Get the Flag

The flag is:

```text
picoCTF{puzzl3d_m3tadata_f0und!_c999e2a4}
```

---

## Flag

```text
picoCTF{puzzl3d_m3tadata_f0und!_c999e2a4}
```

---

## Notes / Lessons Learned

* Do not focus only on the visible content of a file.
* For PDF challenges, always inspect metadata using tools like `pdfinfo` or `exiftool`.
* If you find a suspicious random-looking string, consider common encodings such as Base64.
