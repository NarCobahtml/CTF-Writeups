# BabyLinux5

**Category:** Misc
**Points:** 150
**Author:** zfernm

---

## Description

> Only Learning Basic Linux
>
> BLACKLIST = ["ls", "cat", "head", "less", "more", "awk", "sed", "nl", "strings"]
>
> nc 51.79.201.156 9004

---

## Official Hints

<details>
<summary>Click to show hints</summary>

- No hints available

</details>

---

## Initial Analysis

This challenge further tightens restrictions by blacklisting many common file inspection and text-processing tools.

Key observations:

- Multiple common utilities are blocked (`ls`, `cat`, `awk`, `sed`, etc.)
- The environment is still an interactive shell
- The objective remains to read `flag.txt`

---

## Recon / Enumeration

Connect to the service:

```bash
nc 51.79.201.156 9004
```

Check user:

```bash
whoami
```

Output:

```text
root
```

List files using an alternative command:

```bash
dir
```

Output:

```text
app.py  flag.txt
```

---

## Exploitation

### Step 1 - Bypass File Listing Restriction

Even though `ls` is blocked, `dir` is still available and works as a substitute.

---

### Step 2 - Bypass File Reading Restrictions

Most common file reading and text processing tools are blacklisted:

- `cat`, `head`, `less`, `more`
- `awk`, `sed`, `nl`, `strings`

However, `tac` is not included in the blacklist and can still be used to read files.

```bash
tac flag.txt
```

---

### Step 3 - Get the Flag

The command returns:

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

- Expanding blacklists still leave gaps that can be exploited
- `tac` remains a reliable alternative when common readers are blocked
- Attackers can leverage lesser-known commands to bypass restrictions
- Blacklisting is not a secure defense mechanism
- Understanding multiple Linux utilities is critical in constrained environments
