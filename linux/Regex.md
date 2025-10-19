# 🧩 Linux `grep -E` Regex Practice Guide

## 📘 File Setup
Create a file to practice on:
```bash
cat <<'EOF' > file.txt
# System users
root:x:0:0:root:/root:/bin/bash
amro:x:1000:1000:Amro User:/home/amro:/bin/bash
john:x:1001:1001:John Doe:/home/john:/bin/sh
guest:x:1002:1002:Guest:/home/guest:/usr/sbin/nologin

# Emails
admin@example.com
user123@mail.org
contact@my-site.net
support@company.co.uk

# Logs
INFO  2025-10-18 12:03:45 - System started
WARN  2025-10-18 12:05:01 - Low memory
ERROR 2025-10-18 12:06:12 - Failed to start service
INFO  2025-10-18 12:10:44 - Connection established
DEBUG 2025-10-18 12:15:55 - Debugging mode enabled

# Random Text
My favorite color is blue.
My neighbour's favourite colour is green.
The quick brown fox jumps over 123 lazy dogs.
EOF
```
| #      | Description                                               | Command                                                             |                            |
| ------ | --------------------------------------------------------- | ------------------------------------------------------------------- | -------------------------- |
| **1**  | Lines containing the word “ERROR”                         | `grep -E 'ERROR' file.txt`                                          |                            |
| **2**  | Lines starting with “INFO”                                | `grep -E '^INFO' file.txt`                                          |                            |
| **3**  | Lines ending with “bash”                                  | `grep -E 'bash$' file.txt`                                          |                            |
| **4**  | Lines that contain a number                               | `grep -E '[0-9]+' file.txt`                                         |                            |
| **5**  | Lines containing an email address                         | `grep -E '[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}' file.txt` |                            |
| **6**  | Lines containing “color” or “colour”                      | `grep -E 'colou?r' file.txt`                                        |                            |
| **7**  | Lines with uppercase words only (e.g., INFO, WARN, ERROR) | `grep -E '^[A-Z]+' file.txt`                                        |                            |
| **8**  | Lines with lowercase words only                           | `grep -E '^[a-z ]+$' file.txt`                                      |                            |
| **9**  | Lines with both letters and digits                        | `grep -E '[A-Za-z].*[0-9]                                           | [0-9].*[A-Za-z]' file.txt` |
| **10** | Lines between 20–40 characters long                       | `grep -E '^.{20,40}$' file.txt`                                     |                            |
| **11** | Lines that contain “amro” (case-insensitive)              | `grep -Ei 'amro' file.txt`                                          |                            |
| **12** | Lines that do **not** contain the word “INFO”             | `grep -Ev 'INFO' file.txt`                                          |                            |
| **13** | Lines starting with `#` (comments only)                   | `grep -E '^#' file.txt`                                             |                            |
| **14** | Empty (blank) lines                                       | `grep -E '^$' file.txt`                                             |                            |
| **15** | **Remove all comments and blank lines** ✅                 | `grep -Ev '^(#                                                      | $)' file.txt`              |

💡 Notes

^ → start of line

$ → end of line

. → any single character

* → zero or more

+ → one or more

? → zero or one (optional)

| → OR

[] → character set

() → grouping

{n,m} → repeat between n and m times

-v → invert match (show non-matching lines)

-i → ignore case

-E → use Extended Regular Expressions (modern and recommended)
