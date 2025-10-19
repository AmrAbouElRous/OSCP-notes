# 📘 File Setup

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

---
# 🔍 `grep -E` Regex Practice Examples

| # | Description | Command |
|---|--------------|----------|
| 1 | Find lines containing the word “ERROR” | `grep -E 'ERROR' file.txt` |
| 2 | Find lines starting with “INFO” | `grep -E '^INFO' file.txt` |
| 3 | Find lines ending with “bash” | `grep -E 'bash$' file.txt` |
| 4 | Find lines that contain a number | `grep -E '[0-9]+' file.txt` |
| 5 | Find lines containing an email address | `grep -E '[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}' file.txt` |
| 6 | Find lines containing “color” or “colour” | `grep -E 'colou?r' file.txt` |
| 7 | Find lines with uppercase words only (like INFO, WARN, ERROR) | `grep -E '^[A-Z]+$' file.txt` |
| 8 | Find lines with lowercase words only | `grep -E '^[a-z ]+$' file.txt` |
| 9 | Find lines that contain both letters and digits | `grep -E '([A-Za-z].*[0-9]\|[0-9].*[A-Za-z])' file.txt` |
| 10 | Find lines between 20–40 characters long | `grep -E '^.{20,40}$' file.txt` |
| 11 | Find lines that contain “amro” (case-insensitive) | `grep -Ei 'amro' file.txt` |
| 12 | Show lines that do not contain the word “INFO” | `grep -Ev 'INFO' file.txt` |
| 13 | Show lines starting with # (comments only) | `grep -E '^#' file.txt` |
| 14 | Show empty (blank) lines | `grep -E '^$' file.txt` |
| 15 | Remove all comments and blank lines ✅ | `grep -Ev '^(#\|$)' file.txt` |
| 16 | Show lines that start with a digit | `grep -E '^[0-9]' file.txt` |

---

# 💡 Notes

## Regex Anchors and Symbols

| Symbol | Meaning | Example |
| ------- | -------- | -------- |
| `^` | Start of line | `^INFO` → lines starting with INFO |
| `$` | End of line | `bash$` → lines ending with “bash” |
| `.` | Any single character | `a.c` → matches “abc”, “axc” |
| `*` | Zero or more | `ba*` → “b”, “ba”, “baa”, etc. |
| `+` | One or more | `ba+` → “ba”, “baa”, etc. |
| `?` | Zero or one (optional) | `colou?r` → “color” or “colour” |
| `\|` | OR operator | `foo|bar` → “foo” or “bar” |
| `[]` | Character set | `[abc]` → “a”, “b”, or “c” |
| `()` | Grouping | `(abc)+` → repeat “abc” |
| `{n,m}` | Repeat between n and m times | `a{2,4}` → “aa”, “aaa”, “aaaa” |

---

## `grep` Flags

| Flag | Description |
| ---- | ------------ |
| `-E` | Use Extended Regular Expressions (modern and recommended) |
| `-v` | Invert match (show non-matching lines) |
| `-i` | Ignore case |

---

## Special Sequences (Shortcut Classes)

| Token | Matches | Equivalent |
| ------ | -------- | --------- |
| `\d` | Any digit | `[0-9]` |
| `\D` | Any non-digit | `[^0-9]` |
| `\w` | Any “word” character (letters, digits, underscore) | `[A-Za-z0-9_]` |
| `\W` | Any non-word character | `[^A-Za-z0-9_]` |
| `\s` | Any whitespace (space, tab, newline) | `[ \t\r\n\f]` |
| `\S` | Any non-whitespace | `[^ \t\r\n\f]` |
| `\b` | Word boundary | — |
| `\B` | Non-word boundary | — |

---

## Escaping Special Characters

Some characters have **special meanings** in regex:  
`.` `*` `+` `?` `|` `^` `$` `()` `[]` `{}` `\`

If you want to match them **literally**, you must **escape them with `\`**.

### 💡 Example
```bash
# Match any file containing "version 1.0" literally
grep -E 'version 1\.0' file.txt

# Match literal dollar sign
grep -E '\$HOME' file.txt
```
---

### Additional examples

#### `^$`
matches a blank (empty) line — here’s why:

`^` → means start of line

`$` → means end of line

When they appear right next to each other, it means:

“A line that starts and ends immediately — with nothing in between`

```bash
cat file.txt | grep -Ev '^$' # delete blank lines
cat file.txt | grep -Ev '^#' # delete all comments
cat file.txt | grep -Ev '^(#|$)' #delete all blank lines and comments
cat file.txt | grep -Ev '^(#|$)' | sort | uniq 
```

