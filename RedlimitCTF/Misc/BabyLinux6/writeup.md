# BabyLinux6

**Category:** Misc
**Points:** 150
**Author:** zfernm

---

## Description

> Only Learning Basic Linux
>
> BLACKLIST = ["ls", "cat", "head", "less", "more", "awk", "sed", "nl", "strings", "echo", "printf"]
>
> nc 51.79.201.156 9005

---

## Official Hints

<details>
<summary>Click to show hints</summary>

- No hints available

</details>

---

## Initial Analysis

This challenge adds even more restrictions by blocking output-related commands such as `echo` and `printf`, in addition to common file reading utilities.

Key observations:

- Many standard commands are blacklisted
- The shell is still interactive
- The objective remains to retrieve the flag from `flag.txt`

---

## Recon / Enumeration

Connect to the service:

```bash
nc 51.79.201.156 9005
```

Check user:

```bash
whoami
```

Output:

```text
root
```

List files:

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

Although `ls` is blocked, `dir` can still be used to list files.

---

### Step 2 - Bypass File Reading Restrictions

Common file readers and output commands are blocked:

- `cat`, `head`, `less`, `more`
- `awk`, `sed`, `nl`, `strings`
- `echo`, `printf`

However, `tac` is still allowed and can be used to read the file contents.

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

- Even with extended blacklists, overlooked commands can still be exploited
- `tac` continues to be an effective bypass technique
- Blocking output commands like `echo` and `printf` is not sufficient
- Security through blacklisting is inherently weak
- Knowledge of alternative Linux tools is crucial in restricted shells
