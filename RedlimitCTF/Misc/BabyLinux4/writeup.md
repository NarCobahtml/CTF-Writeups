# BabyLinux4

**Category:** Misc
**Points:** 50
**Author:** zfernm

---

## Description

> Only Learning Basic Linux
>
> BLACKLIST = ["ls", "cat", "head", "less", "more"]
>
> nc 51.79.201.156 9003

---

## Official Hints

<details>
<summary>Click to show hints</summary>

- No hints available

</details>

---

## Initial Analysis

This challenge increases restrictions by blacklisting multiple common commands.

Key observations:

- `ls`, `cat`, `head`, `less`, and `more` are blocked
- We still have an interactive shell
- We must find alternative commands to list files and read contents

---

## Recon / Enumeration

Connect to the service:

```bash
nc 51.79.201.156 9003
```

Check current user:

```bash
whoami
```

Output:

```text
root
```

List files using alternative command:

```bash
dir
```

Output:

```text
HIDUP_JOKOWI  app.py  asd  file.sh  flag.txt  hacked.txt  ls  s
```

Attempting to use other tools like `file` fails:

```bash
file HIDUP_JOKOWI
```

Output:

```text
/bin/sh: 1: file: not found
```

---

## Exploitation

### Step 1 - Bypass File Listing Restriction

Even though `ls` is blocked, `dir` is still allowed and can be used to list files.

---

### Step 2 - Bypass File Reading Restriction

Common file readers like `cat`, `head`, `less`, and `more` are blocked.

However, `tac` is not blacklisted and can be used to read file contents.

```bash
tac flag.txt
```

`tac` outputs the file in reverse line order, but since the flag is a single line, it works perfectly.

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

- Blacklists are often incomplete and can be bypassed with alternative commands
- `dir` is a useful replacement for `ls`
- `tac` can bypass restrictions on `cat` and similar tools
- Always try less common utilities when standard ones are blocked
- Understanding Linux command alternatives is crucial in restricted environments
