# Log Hunt

**Category:** General Skills
**Points:** Unknown
**Author:** Yahaya Meddy

---

## Description

> Our server seems to be leaking pieces of a secret flag in its logs. The parts are scattered and sometimes repeated. Can you reconstruct the original flag?
>
> Download the logs and figure out the full flag from the fragments.

---

## Official Hints

<details>
<summary>Click to show hints</summary>

* Hint 1: You can use grep to filter only matching lines from the log.
* Hint 2: Some lines are duplicates; ignore extra occurrences.

</details>

---

## Initial Analysis

This challenge provides a log file named `server.log`.  
The goal is to recover the flag hidden inside the log data.

The file is extremely large (the provided snippet is only a small portion), so manual inspection is inefficient.

---

## Recon / Enumeration

First, verify the file type:

```bash
file server.log
```

Output:

```text
server.log: ASCII text
```

Display the contents:

```bash
cat server.log
```

Partial output:

```text
[1990-08-09 10:00:16] WARN Disk space low
[1990-08-09 10:00:19] DEBUG Cache cleared
[1990-08-09 10:00:23] WARN Disk space low
[1990-08-09 10:00:25] INFO Service restarted
[1990-08-09 10:00:33] WARN Disk space low
[1990-08-09 10:00:38] ERROR Connection lost
[1990-08-09 10:00:46] ERROR Failed login attempt
[1990-08-09 10:00:48] INFO User logged in
[1990-08-09 10:00:50] INFO User logged in
[1990-08-09 10:00:59] ERROR Failed login attempt
[1990-08-09 10:01:04] DEBUG System check complete
[1990-08-09 10:01:05] WARN Disk space low
[1990-08-09 10:01:13] INFO Service restarted
[1990-08-09 10:01:17] INFO Service restarted
[1990-08-09 10:01:18] ERROR Failed login attempt
[1990-08-09 10:01:24] ERROR Connection lost
[1990-08-09 10:01:25] WARN High memory usage detected
[1990-08-09 10:01:31] ERROR Connection lost
[1990-08-09 10:01:36] INFO Service restarted
[1990-08-09 10:01:41] ERROR Connection lost
[1990-08-09 10:01:44] WARN High memory usage detected
[1990-08-09 10:01:53] INFO Scheduled task run
[1990-08-09 10:01:54] DEBUG System check complete
[1990-08-09 10:01:57] DEBUG System check complete
[1990-08-09 10:01:58] INFO Scheduled task run
[1990-08-09 10:02:04] WARN Disk space low
[1990-08-09 10:02:07] INFO Service restarted
[1990-08-09 10:02:16] ERROR Failed login attempt
[1990-08-09 10:02:26] DEBUG Cache cleared
[1990-08-09 10:02:30] DEBUG System check complete
[1990-08-09 10:02:37] ERROR Connection lost
[1990-08-09 10:02:45] INFO User logged in
[1990-08-09 10:03:03] WARN High memory usage detected
[1990-08-09 10:03:09] ERROR Connection lost
...
```

The log file is **extremely long**, so manual inspection is not practical.

---

## Exploitation

### Step 1 - Find the Vulnerability

To handle large files efficiently, use `less`:

```bash
strings server.log | less
```

This allows scrolling through the file page by page.

From here, we can already spot lines containing `FLAGPART`.

### Step 2 - Develop the Exploit

Instead of manually searching, use `grep`:

```bash
strings server.log | grep "FLAGPART"
```

Output:

```text
[1990-08-09 10:00:10] INFO FLAGPART: picoCTF{us3_
[1990-08-09 10:02:55] INFO FLAGPART: y0urlinux_
[1990-08-09 10:05:54] INFO FLAGPART: sk1lls_
[1990-08-09 10:05:55] INFO FLAGPART: sk1lls_
[1990-08-09 10:10:54] INFO FLAGPART: cedfa5fb}
...
```

We observe:

* The flag is split into multiple parts (`FLAGPART`)
* Some entries are duplicated

Unique parts:

```text
picoCTF{us3_
y0urlinux_
sk1lls_
cedfa5fb}
```

Combine:

```text
picoCTF{us3_y0urlinux_sk1lls_cedfa5fb}
```

### Step 3 - Get the Flag

The reconstructed flag is:

```text
picoCTF{us3_y0urlinux_sk1lls_cedfa5fb}
```

---

## Flag

```text
picoCTF{us3_y0urlinux_sk1lls_cedfa5fb}
```

---

## Notes / Lessons Learned

* Large log files should be analyzed with tools like `less` and `grep`
* Focus on meaningful patterns (`FLAGPART`)
* Extract → deduplicate → reconstruct
