# BabyLinux3

**Category:** Misc
**Points:** 50
**Author:** zfernm

---

## Description

> Only Learning Basic Linux

The challenge provides a remote Linux shell with a simple blacklist that blocks `ls` and `cat`.

---

## Official Hints

<details>
<summary>Click to show hints</summary>

* No hints available

</details>

---

## Initial Analysis

The challenge is a basic Linux command execution task.

What stands out immediately:

* It is a remote service accessible through `nc`.
* The environment is interactive and accepts shell commands.
* `ls` and `cat` are blacklisted, so the intended solution is likely to use alternative Linux commands.

---

## Recon / Enumeration

```bash
nc 51.79.201.156 9002
```

After connecting, the service prints a welcome message and asks for a command.

Useful observations from quick testing:

* `whoami` works and shows the session runs as `root`.
* `dir` can be used instead of `ls` to list files.
* The directory contains `app.py` and `flag.txt`.

Because `cat` is blacklisted, we need another way to read the flag file.

---

## Exploitation

### Step 1 - Find a Way Around the Blacklist

The blacklist only blocks specific commands, not every file-reading utility.

Since `tac` is not blocked, it can be used to display the contents of `flag.txt`.

---

### Step 2 - Read the Flag

```bash
tac flag.txt
```

`tac` prints the file contents in reverse line order. For a single-line flag file, this still reveals the full flag directly.

---

### Step 3 - Get the Flag

Running the command returns the flag:

```text
REDLIMIT{547225a499947bc7170c9a00f45295b06d69c9cefe489da0b44ab54cc1b5b906}
```

---

## Flag

```text
REDLIMIT{547225a499947bc7170c9a00f45295b06d69c9cefe489da0b44ab54cc1b5b906}
```

---

## Notes / Lessons Learned

* Command blacklists are weak defenses because alternative utilities often provide the same functionality.
* `dir` is a practical replacement for `ls`.
* `tac` is a simple bypass when `cat` is blocked.
