# B64

**Category:** Crypto
**Points:** 50
**Author:** zfernm

---

## Description

> Team SOC mendapatkan temuan malware dan didalam malware tersebut ada enkripsi berupa seperti ini `UkVETElNSVR7QkFTRTY0RU5DT0RJTkd9` bantu saya menemukan enkripsi dan plaintext nya

---

## Official Hints

<details>
<summary>Click to show hints</summary>

- No hints available

</details>

---

## Initial Analysis

The challenge provides a single encoded string:

```
UkVETElNSVR7QkFTRTY0RU5DT0RJTkd9
```

Observations:

- The string consists of alphanumeric characters typical of Base64
- The challenge mentions encryption, but this is actually encoding
- Likely a straightforward Base64 decoding task

---

## Recon / Enumeration

Test decoding the string:

```bash
echo UkVETElNSVR7QkFTRTY0RU5DT0RJTkd9 | base64 -d
```

Output:

```text
REDLIMIT{BASE64ENCODING}
```

---

## Exploitation

### Step 1 - Identify Encoding

The string matches Base64 format due to its character set and structure.

---

### Step 2 - Decode the String

Use the `base64` tool:

```bash
echo UkVETElNSVR7QkFTRTY0RU5DT0RJTkd9 | base64 -d
```

---

### Step 3 - Get the Flag

The decoded result is:

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

- Base64 is encoding, not encryption
- Recognizing encoding patterns is important in CTFs
- Simple tools can quickly solve basic crypto challenges
