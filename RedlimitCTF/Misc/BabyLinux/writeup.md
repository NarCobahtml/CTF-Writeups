# BabyLinux

**Category:** Misc
**Points:** 50
**Author:** zfernm

---

## Description

> Only Learning Basic Linux
>
> nc 51.79.201.156 9000

---

## Official Hints

<details>
<summary>Click to show hints</summary>

- No hints available

</details>

---

## Initial Analysis

This challenge provides a remote Linux shell accessible via netcat.

Key observations:

- The service is interactive
- It accepts basic Linux commands
- No obvious restrictions are mentioned
- Likely intended for beginners to explore basic commands

---

## Recon / Enumeration

Connect to the service:

```bash
nc 51.79.201.156 9000
```

After connecting, we get:

```text
Welcome To CTF REDLIMIT X META4SEC Basic Linux Chall!
Type your command:
```

Start by checking the current user:

```bash
whoami
```

Output:

```text
root
```

We have root access.

List files in the directory:

```bash
ls
```

Output:

```text
app.py
flag.txt
```

---

## Exploitation

### Step 1 - Identify Available Files

The directory contains a file named `flag.txt`, which is likely the target.

---

### Step 2 - Read the Flag File

Since there are no restrictions, we can directly read the file using `cat`:

```bash
cat flag.txt
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

- Basic Linux commands like `ls`, `whoami`, and `cat` are essential in CTFs
- Always check accessible files in a directory
- Many beginner challenges focus on understanding environment interaction
- If no restrictions are applied, the simplest solution is often the correct one
