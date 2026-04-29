# BabyLinux2

**Category:** Misc
**Points:** 50
**Author:** zfernm

---

## Description

> Only Learning Basic Linux
>
> BLACKLIST = ["ls"]
>
> nc 51.79.201.156 9001

---

## Official Hints

<details>
<summary>Click to show hints</summary>

- No hints available

</details>

---

## Initial Analysis

This challenge is similar to the previous BabyLinux challenge, but with a restriction.

Key observations:

- The service is accessible via netcat
- The command `ls` is explicitly blacklisted
- We still have an interactive shell
- The goal remains to retrieve the flag

---

## Recon / Enumeration

Connect to the service:

```bash
nc 51.79.201.156 9001
```

After connecting:

```text
Welcome To CTF REDLIMIT X META4SEC Basic Linux Chall!
Type your command:
```

Check user:

```bash
whoami
```

Output:

```text
root
```

Attempt to list files:

```bash
ls
```

Output:

```text
Command not allowed!
```

Since `ls` is blocked, try an alternative:

```bash
dir
```

Output:

```text
app.py  flag.txt
```

---

## Exploitation

### Step 1 - Bypass the Blacklist

The blacklist only blocks the `ls` command, not other file listing utilities.

Using `dir` allows us to list directory contents successfully.

---

### Step 2 - Read the Flag File

Now that we know `flag.txt` exists, we can read it using `cat`:

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

- Command blacklists are often incomplete and can be bypassed
- `dir` can be used as an alternative to `ls`
- Always try similar or equivalent commands when one is blocked
- Understanding basic Linux utilities is essential in CTF challenges
