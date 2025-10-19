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

# 🔍 `grep -E` Regex Practice Examples

## 1. Find lines containing the word “ERROR”
```bash
grep -E 'ERROR' file.txt```
## 2. Find lines starting with “INFO”
```bash
grep -E '^INFO' file.txt```
## 3. Find lines ending with “bash”
```bash
grep -E 'bash$' file.txt```
## 4. Find lines that contain a number
```bash
grep -E '[0-9]+' file.txt```
## 5. Find lines containing an email address
```bash
grep -E '[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}' file.txt```
## 6. Find lines containing “color” or “colour”
```bash
grep -E 'colou?r' file.txt```
## 7. Find lines with uppercase words only (like INFO, WARN, ERROR)
```bash
grep -E '^[A-Z]+$' file.txt```
## 8. Find lines with lowercase words only
```bash
grep -E '^[a-z ]+$' file.txt```
## 9. Find lines that contain both letters and digits
```bash
grep -E '([A-Za-z].*[0-9]|[0-9].*[A-Za-z])' file.txt```
## 10. Find lines between 20–40 characters long
```bash
grep -E '^.{20,40}$' file.txt```
## 11. Find lines that contain “amro” (case-insensitive)
```bash
grep -Ei 'amro' file.txt```
## 12. Show lines that do not contain the word “INFO”
```bash
grep -Ev 'INFO' file.txt```
## 13. Show lines starting with # (comments only)
```bash
grep -E '^#' file.txt```
## 14. Show empty (blank) lines
```bash
grep -E '^$' file.txt```
## 15. Remove all comments and blank lines ✅
```bash
grep -Ev '^(#|$)' file.txt```
## 16. Show lines that start with a digit
```bash
grep -E '^[0-9]' file.txt
```
#💡 Notes
## ^ → start of line (example: ^INFO matches lines that begin with INFO)

## ^ inside a character class (first character) → negation (example: [^0-9] matches any character that is not a digit)

## $ → end of line

## . → any single character

## * → zero or more

## + → one or more

## ? → zero or one (optional)

## | → OR

## [] → character set (e.g., [abc])

## () → grouping

## {n,m} → repeat between n and m times

## -v → invert match (show non-matching lines)

## -i → ignore case

## -E → use Extended Regular Expressions (modern and recommended)

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
