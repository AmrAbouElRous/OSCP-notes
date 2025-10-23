# sed Command Cheat Sheet

A quick reference for commonly used **`sed` (Stream Editor)** commands in Linux.

---

## 🧠 Basic Syntax
```bash
sed [options] 'command' filename
```
Or use with a pipe:
```bash
cat file | sed 'command'
```

---

## 🔍 Viewing / Printing

### Print specific lines
```bash
sed -n '3p' file.txt        # Print only line 3
sed -n '1,5p' file.txt      # Print lines 1 to 5
sed -n '/error/p' file.txt  # Print lines containing "error"
```

### Print all except certain lines
```bash
sed '3d' file.txt           # Delete line 3 (doesn't change file)
sed '1,5d' file.txt         # Delete lines 1 to 5
sed '/error/d' file.txt     # Delete lines containing "error"
```

---

## ✍️ Editing / Substitution

### Replace first occurrence in each line
```bash
sed 's/foo/bar/' file.txt
```

### Replace all occurrences in each line
```bash
sed 's/foo/bar/g' file.txt
```

### Replace only on specific lines
```bash
sed '3s/foo/bar/' file.txt    # Replace on line 3
sed '1,5s/foo/bar/g' file.txt # Replace lines 1–5
```

### Replace with regex
```bash
sed -E 's/[0-9]+/NUMBER/g' file.txt
```

### Replace in-place (edit the file directly)
```bash
sed -i 's/foo/bar/g' file.txt
```
> Create a backup before editing:
> ```bash
> sed -i.bak 's/foo/bar/g' file.txt
> ```

---

## ➕ Inserting / Appending / Changing

### Insert text before a line
```bash
sed '3i\\This is inserted text' file.txt
```

### Append text after a line
```bash
sed '3a\\This goes after line 3' file.txt
```

### Replace an entire line
```bash
sed '3c\\This line replaces line 3' file.txt
```

---

## ⚙️ Using Patterns

### Delete lines matching a pattern
```bash
sed '/^$/d' file.txt        # Remove empty lines
sed '/^#/d' file.txt        # Remove comments (lines starting with #)
```

### Keep only lines that match a pattern
```bash
sed -n '/pattern/p' file.txt
```

### Remove trailing or leading spaces
```bash
sed 's/[[:space:]]*$//' file.txt  # Remove trailing spaces
sed 's/^[[:space:]]*//' file.txt  # Remove leading spaces
```

---

## 🧩 Multiple Commands

### Run multiple edits
```bash
sed -e 's/foo/bar/g' -e '/^$/d' file.txt
```

### Use a script block
```bash
sed '
/error/d
s/fail/FAIL/g
' file.txt
```

---

## 💡 Extra Tricks

### Show line numbers
```bash
sed = file.txt | sed 'N;s/\n/ /'
```

### Replace only the nth occurrence on a line
```bash
sed 's/foo/bar/2' file.txt   # Replace 2nd “foo” only
```

### Replace between patterns
```bash
sed '/START/,/END/s/foo/bar/g' file.txt
```

### Delete between two patterns
```bash
sed '/BEGIN/,/END/d' file.txt
```

---

## 🧮 Regex Basics for `sed`

| Symbol | Meaning | Example |
|---------|----------|----------|
| `^` | Start of line | `^Hello` matches lines starting with "Hello" |
| `$` | End of line | `world$` matches lines ending with "world" |
| `.` | Any single character | `h.t` matches "hat", "hit", etc. |
| `*` | Zero or more of previous | `go*` matches "g", "go", "goo" |
| `+` | One or more of previous | `go+` matches "go", "goo" |
| `?` | Zero or one of previous | `colou?r` matches "color" or "colour" |
| `[]` | Character class | `[aeiou]` matches vowels |
| `[^]` | Negated class | `[^0-9]` matches non-digits |
| `{m,n}` | Range of repetitions | `[0-9]{3}` matches 3 digits |
| `()` | Grouping | `(abc)+` matches one or more repetitions of "abc" |
| `|` | OR operator | `foo|bar` matches "foo" or "bar" |

---

### 🧾 Notes
- `-n` suppresses automatic printing.
- `-i` edits files **in place**.
- `-E` enables extended regular expressions.

---
**Created for quick daily use by Linux users and Bash scripters.**

