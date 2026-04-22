# Flag in Flame

**Category:** Forensic
**Points:** Unknown
**Author:** Prince Niyonshuti N.

---

## Description

> The SOC team discovered a suspiciously large log file after a recent breach. When they opened it, they found an enormous block of encoded text instead of typical logs. Could there be something hidden within? Your mission is to inspect the resulting file and reveal the real purpose of it. The team is relying on your skills to uncover any concealed information within this unusual log.
>
> Download the encoded data here: Logs Data. Be prepared-the file is large, and examining it thoroughly is crucial.

---

## Official Hints

<details>
<summary>Click to show hints</summary>

* Hint 1: Use base64 to decode the data and generate the image file.

</details>

---

## Initial Analysis

The challenge gives a very large text file named `logs.txt`.

At first glance it looks like logs, but the contents are actually a long Base64 string. The hint already points to the right direction: decode the data and turn it into an image.

---

## Recon / Enumeration

Check the file type and preview the beginning of the file:

```bash
file logs.txt
wc -c logs.txt
head -c 200 logs.txt
```

Important result:

```text
logs.txt: ASCII text, with very long lines, with no line terminators
```

The file starts with:

```text
iVBORw0KGgoAAAANSUhEUg...
```

That is the typical prefix of a Base64-encoded PNG file.

---

## Exploitation

### Step 1 - Decode the Base64 Data

Convert the Base64 text into a PNG image:

```bash
base64 -d logs.txt > flame.png
```

Verify the output:

```bash
file flame.png
```

Result:

```text
flame.png: PNG image data, 896 x 1152, 8-bit/color RGB, non-interlaced
```

### Step 2 - Open the Image

Open the generated image to inspect it visually:

```bash
xdg-open flame.png
```

The image reveals a hexadecimal string at the bottom. Decoding it gives the final flag.

The hex string is:

```text
7069636F4354467B666F72656E736963735F616E616C797369735F616D617A696E675F62653836303237397D
```

Decode it:

```bash
python3 - <<'PY'
s='7069636F4354467B666F72656E736963735F616E616C797369735F616D617A696E675F62653836303237397D'
print(bytes.fromhex(s).decode())
PY
```

Result:

```text
picoCTF{forensics_analysis_is_amazing_be860279}
```

---

## Flag

```text
picoCTF{forensics_analysis_is_amazing_be860279}
```

---

## Notes / Lessons Learned

* Large "log" files can actually be encoded payloads.
* A Base64 prefix such as `iVBORw0KGgo` usually means PNG data.
* Decoding Base64 and opening the generated file is often enough in simple forensic challenges.
