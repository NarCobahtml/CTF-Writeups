# Corrupted JPEG Header

**Category:** Forensic
**Points:** Unknown

---

## Description

> This file seems broken... or is it? Maybe a couple of bytes could make all the difference. Can you figure out how to bring it back to life?
>
> Download the file here.

---

## Official Hints

<details>
<summary>Click to show hints</summary>

* Hint 1: Try checking the file’s header.
* Hint 2: JPEG
* Hint 3: Tools like xxd or hexdump can help you inspect and edit file bytes.

</details>

---

## Initial Analysis

This challenge provides a file named `file.1` that cannot be opened normally. The hint suggests that the file is related to JPEG, indicating that the file may be corrupted or missing a proper header.

---

## Recon / Enumeration

Check the file type:

```bash
file file.1
```

Output:

```text
file.1: data
```

The file is not recognized as a valid format, so further inspection is required.

Inspect the file using a hex dump:

```bash
xxd file.1
```

Output (partial):

```text
00000000: 5c78 ffe0 0010 4a46 4946 0001 0100 0001
```

Observation:

* The file contains `JFIF`, which is commonly found in JPEG files
* However, the file does **not** start with the correct JPEG magic bytes

---

## Exploitation

### Step 1 - Find the Vulnerability

A valid JPEG file should start with:

```text
FF D8
```

But instead, the file starts with:

```text
5c78
```

This indicates that the header is corrupted or incorrectly written.

### Step 2 - Develop the Exploit

Overwrite the first two bytes with the correct JPEG magic bytes:

```bash
printf '\xff\xd8' | dd of=file.1 bs=1 count=2 conv=notrunc
```

Explanation:

* `\xff\xd8` → correct JPEG header
* `dd` writes directly to the file without truncating it

### Step 3 - Get the Flag

Check the file type again:

```bash
file file.1
```

Output:

```text
file.1: JPEG image data, JFIF standard 1.01, 800x500
```

The file is now recognized as a valid JPEG image.

Open the file:

```bash
xdg-open file.1
```

The image will display the flag.

---

## Flag

```text
picoCTF{r3st0r1ng_th3_by73s_1512b52a}
```

---

## Notes / Lessons Learned

* Always check file signatures (magic bytes) when a file is unrecognized
* JPEG files must start with `FF D8`
* Tools like `xxd` are useful for low-level file inspection
* Small byte-level corruption can prevent files from being recognized
* Restoring correct headers can recover hidden data
