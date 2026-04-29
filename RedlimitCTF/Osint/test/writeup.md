# RedLimit OSINT Profile Dump

**Category:** OSINT
**Points:** 50
**Author:** admin

---

## Description

> === [ INTERCEPTED PROFILE DUMP: RedLimit ] ===
>
> Username: @RedLimit
> Status: "Mencari kebenaran di tumpukan jerami digital."
> Bio:
> "Suka kriptografi dan fotografi.
> Jangan coba-coba meretas saya.
> Part 1: UkVETElNSVR7"
>
> Recent Activity Log:
> [2023-10-27 14:00] Uploaded file: 'secret_view.jpg' (deleted)
> [2023-10-27 14:05] Updated status: "Koordinatku ada di mana-mana."
> [2023-10-27 14:10] Posted comment:
> "Guys, aku lupa password emailku. Hint-nya adalah nama kucingku ditambah underscore.
> Kucingku namanya 'OS1NT'.
> Btw, Part 2 kuncinya ada di sini tapi terbalik: \_TNSI"
>
> [2023-10-27 14:15] Emergency Note saved:
> "Ingat, bagian terakhir selalu yang paling sulit.
> Gunakan Hex untuk bicara denganku.
> Part 3: 5930557D"

---

## Official Hints

<details>
<summary>Click to show hints</summary>

- No hints available

</details>

---

## Initial Analysis

The challenge provides a profile dump containing multiple clues encoded in different formats.

Key observations:

- Three parts are explicitly labeled (Part 1, Part 2, Part 3)
- Each part uses a different encoding method
- There is an additional hint about a cat name: `OS1NT`
- The structure suggests the final flag is constructed by combining all decoded parts

---

## Recon / Enumeration

We decode each part using common techniques.

### Decode Part 1 (Base64)

```bash
base64 -d <<< UkVETElNSVR7
```

Output:

```text
REDLIMIT{
```

---

### Reverse Part 2

```text
_TNSI → ISNT_
```

---

### Decode Part 3 (Hex)

```bash
echo 5930557D | xxd -r -p
```

Output:

```text
Y0U}
```

---

## Exploitation

### Step 1 - Understand the Clues

From decoding we get:

- `REDLIMIT{`
- `ISNT_`
- `Y0U}`

Additionally, a crucial hint:

- Cat name: `OS1NT`
- Instruction: "cat name + underscore"

---

### Step 2 - Construct the Flag

Combine all components in a meaningful order:

```text
REDLIMIT{OS1NT_ISNT_Y0U}
```

---

### Step 3 - Get the Flag

Submitting the constructed flag to the platform confirms that it is correct.

---

## Flag

```text
REDLIMIT{REDACTED}
```

---

## Notes / Lessons Learned

- Multiple encoding techniques can be combined in a single challenge
- Always check for additional hints outside labeled parts
- Reversed strings and hex encoding are common in CTFs
- Context clues (like names) are often required to complete the flag
- OSINT challenges often focus on attention to detail rather than complex exploitation
