# Hidden in plainsight

**Category:** Forensic
**Points:** Unknown
**Author:** Yahaya Meddy

---

## Description

> You’re given a seemingly ordinary JPG image. Something is tucked away out of sight inside the file. Your task is to discover the hidden payload and extract the flag.
>
> Download the jpg image here.

---

## Official Hints

<details>
<summary>Click to show hints</summary>

* Hint 1: Download the jpg image and read its metadata

</details>

---

## Initial Analysis

We are given an image file named `img.jpg`. The goal is to find the flag hidden inside the file.

---

## Recon / Enumeration

Check the file type and metadata:

```bash
file img.jpg
exiftool img.jpg
```

Important result from the metadata:

```text
Comment : c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9
```

The value in the `Comment` field looks like a Base64 string.

---

## Exploitation

### Step 1 - Find the Vulnerability

Decode the string:

```bash
printf '%s' 'c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9' | base64 -d
```

Result:

```text
steghide:cEF6endvcmQ=
```

This format strongly suggests that the file uses `steghide`, and the part after the `:` is also still Base64-encoded.

Decode the last part again:

```bash
printf '%s' 'cEF6endvcmQ=' | base64 -d
```

Result:

```text
pAzzword
```

So the password for `steghide` is:

```text
pAzzword
```

### Step 2 - Develop the Exploit

Use that password to inspect the embedded file information:

```bash
steghide info -p 'pAzzword' img.jpg
```

Important result:

```text
embedded file "flag.txt"
```

This confirms that there is indeed a hidden file inside the image.

Extract the file contents using `steghide`:

```bash
steghide extract -sf img.jpg -p 'pAzzword' -f
```

Output:

```text
wrote extracted data to "flag.txt".
```

### Step 3 - Get the Flag

View the contents of the extracted file:

```bash
cat flag.txt
```

Result:

```text
picoCTF{h1dd3n_1n_1m4g3_54e31417}
```

---

## Flag

```text
picoCTF{h1dd3n_1n_1m4g3_54e31417}
```

---

## Notes / Lessons Learned

* The JPEG metadata contains a clue encoded in Base64.
* Decoding it reveals that `steghide` is used.
* The `steghide` password is also stored in Base64.
* After extraction, the file `flag.txt` contains the flag.
