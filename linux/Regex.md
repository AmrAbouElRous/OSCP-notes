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
| # | Description | Command |
|---|-------------|---------|
| **1** | Lines containing the word “ERROR” | <code>grep -E 'ERROR' file.txt</code> |
| **2** | Lines starting with “INFO” | <code>grep -E '^INFO' file.txt</code> |
| **3** | Lines ending with “bash” | <code>grep -E 'bash$' file.txt</code> |
| **4** | Lines that contain a number | <code>grep -E '[0-9]+' file.txt</code> |
| **5** | Lines containing an email address | <code>grep -E '[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}' file.txt</code> |
| **6** | Lines containing “color” or “colour” | <code>grep -E 'colou?r' file.txt</code> |
| **7** | Lines with uppercase words only (e.g., INFO, WARN, ERROR) | <code>grep -E '^[A-Z]+$' file.txt</code> |
| **8** | Lines with lowercase words only | <code>grep -E '^[a-z ]+$' file.txt</code> |
| **9** | Lines with both letters and digits | <code>grep -E '([A-Za-z].*[0-9]|[0-9].*[A-Za-z])' file.txt</code> |
| **10** | Lines between 20–40 characters long | <code>grep -E '^.{20,40}$' file.txt</code> |
| **11** | Lines that contain “amro” (case-insensitive) | <code>grep -Ei 'amro' file.txt</code> |
| **12** | Lines that do **not** contain the word “INFO” | <code>grep -Ev 'INFO' file.txt</code> |
| **13** | Lines starting with `#` (comments only) | <code>grep -E '^#' file.txt</code> |
| **14** | Empty (blank) lines | <code>grep -E '^$' file.txt</code> |
| **15** | **Remove all comments and blank lines** ✅ | <code>grep -Ev '^(#|$)' file.txt</code> |
| **16** | Lines that start with a digit | <code>grep -E '^[0-9]' file.txt</code> |


💡 Notes
^ → start of line (example: ^INFO matches lines that begin with INFO)

^ inside a character class (first character) → negation (example: [^0-9] matches any character that is not a digit)

$ → end of line

. → any single character

* → zero or more

+ → one or more

? → zero or one (optional)

| → OR

[] → character set (e.g., [abc])

() → grouping

{n,m} → repeat between n and m times

-v → invert match (show non-matching lines)

-i → ignore case

-E → use Extended Regular Expressions (modern and recommended)

Special Sequences (Shortcut Classes)

| Token | Matches                                            | Equivalent      |
| ----- | -------------------------------------------------- | --------------- |
| `\d`  | Any digit                                          | `[0-9]`         |
| `\D`  | Any non-digit                                      | `[^0-9]`        |
| `\w`  | Any “word” character (letters, digits, underscore) | `[A-Za-z0-9_]`  |
| `\W`  | Any non-word character                             | `[^A-Za-z0-9_]` |
| `\s`  | Any whitespace (space, tab, newline)               | `[ \t\r\n\f]`   |
| `\S`  | Any non-whitespace                                 | `[^ \t\r\n\f]`  |
| `\b`  | Word boundary                                      | —               |
| `\B`  | Non-word boundary                                  | —               |

Some characters have **special meanings** in regex:  
`.` `*` `+` `?` `|` `^` `$` `()` `[]` `{}` `\`

If you want to match them **literally**, you must **escape them with `\`**.
#### 💡 Example in `grep -E`
```bash
# match any file containing "version 1.0" literally
grep -E 'version 1\.0' file.txt

# match literal dollar sign
grep -E '\$HOME' file.txt
