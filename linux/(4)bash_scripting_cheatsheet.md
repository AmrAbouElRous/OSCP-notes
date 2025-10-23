# Bash Scripting Cheat Sheet

A complete reference for writing and understanding Bash scripts — from basics to advanced techniques.

---

## 🧩 Basics
```bash
#!/bin/bash        # Shebang: defines interpreter (bash)
# Comments start with '#'

echo "Hello, World!"   # Print text to terminal
echo $USER             # Print environment variable (e.g., current user)
echo $(pwd)            # Command substitution (prints current directory)
```

**Notes:**
- Use `#!/bin/bash` at the top of every script.
- To run: `chmod +x script.sh && ./script.sh`

---

## 🔢 Variables
```bash
name="Amro"           # Assign a string
age=25                # Assign a number (no spaces around '=')
echo "Hello, $name! You are $age years old."

# Arithmetic operations
num1=10
num2=20
sum=$((num1 + num2))  # Double parentheses for arithmetic
echo "Sum: $sum"

# Command substitution
current_dir=$(pwd)
echo "You are in $current_dir"
```

**Notes:**
- `$variable` to access the value.
- Use `$(command)` or backticks `` `command` `` to capture command output.

---

## ⚙️ Conditionals
```bash
# String comparison
if [ "$USER" == "amro" ]; then
    echo "Welcome, $USER!"
else
    echo "Access denied."
fi

# Integer comparison
if [ $a -gt $b ]; then
    echo "$a is greater than $b"
elif [ $a -eq $b ]; then
    echo "Equal"
else
    echo "$a is smaller than $b"
fi

# File tests
if [ -f "$file" ]; then
    echo "File exists."
fi

if [ -d "$dir" ]; then
    echo "Directory exists."
fi
```

**Common Comparison Operators:**
| Type | Operator | Meaning |
|------|-----------|----------|
| String | `==` | Equal |
| String | `!=` | Not equal |
| Integer | `-eq` | Equal |
| Integer | `-ne` | Not equal |
| Integer | `-gt` | Greater than |
| Integer | `-lt` | Less than |
| File | `-f` | File exists |
| File | `-d` | Directory exists |
| File | `-r` | Readable |
| File | `-w` | Writable |
| File | `-x` | Executable |

---

## 🔁 Loops
```bash
# For loop with range
for i in {1..5}; do
  echo "Number $i"
done

# For loop over files
for file in *.txt; do
  echo "Processing $file"
done

# While loop
count=1
while [ $count -le 5 ]; do
  echo "Count: $count"
  ((count++))   # Increment
  sleep 1       # Pause 1 second
done

# Until loop (runs until condition becomes true)
until [ $count -gt 5 ]; do
  echo "Count: $count"
  ((count++))
done
```

**Notes:**
- `{1..5}` expands to `1 2 3 4 5`.
- Use `break` to exit a loop early, `continue` to skip to next iteration.

---

## 🧠 Functions
```bash
greet() {
  echo "Hello, $1!"
  echo "Your second argument is: $2"
}

greet Amro Bash

# Returning values
double() {
  local result=$(( $1 * 2 ))
  echo $result
}

value=$(double 5)
echo "Double: $value"
```

**Notes:**
- `$1`, `$2`, ... represent positional arguments.
- `local` limits variable scope to inside the function.
- Functions must be defined before they are used.

---

## 🧮 Arguments
```bash
echo "Script name: $0"   # Script filename
echo "First arg: $1"    # First argument
echo "Second arg: $2"
echo "All args: $@"     # All arguments (as list)
echo "Arg count: $#"    # Number of arguments

# Example run:
# ./script.sh apple banana
```

**Notes:**
- `$*` expands all args as a single string.
- `$@` expands all args as separate quoted words.

---

## 💬 Reading Input
```bash
read -p "Enter your name: " username
echo "Hello, $username!"

# Silent password input
read -sp "Enter password: " password
echo "\nPassword received."
```

**Notes:**
- `-p` adds a prompt.
- `-s` hides typed input (useful for passwords).

---

## 📂 File Operations
```bash
# Check if file exists
if [ -f "$filename" ]; then
  echo "File exists."
else
  echo "File not found."
fi

# Create and write to file
echo "Hello World" > output.txt  # Overwrites file
echo "New line" >> output.txt   # Appends to file

# Read file line by line
while IFS= read -r line; do
  echo "$line"
done < "$filename"
```

**Notes:**
- `IFS=` prevents trimming whitespace.
- `-r` preserves backslashes.

---

## 📅 Date and Time
```bash
echo "Current date: $(date)"
echo "Year: $(date +%Y)"
echo "Month: $(date +%m)"
echo "Day: $(date +%d)"
echo "Time: $(date +%H:%M:%S)"
```

**Useful date formats:**
- `%Y` – Year (e.g., 2025)
- `%m` – Month (01–12)
- `%d` – Day (01–31)
- `%H` – Hour (00–23)
- `%M` – Minute
- `%S` – Second

---

## 🧰 Useful Commands
```bash
# Replace text in a file using sed
sed 's/old/new/g' file.txt

# Search for a pattern in a file using grep
grep "pattern" file.txt

# Count lines, words, and characters
wc file.txt

# List only directories
ls -d */

# Sort lines alphabetically
sort file.txt
```

---

## 🔒 Permissions & Execution
```bash
chmod +x script.sh   # Make script executable
./script.sh          # Run script in current directory
bash script.sh       # Run with explicit interpreter
```

**Notes:**
- `chmod +x` gives execute permission.
- Use `bash -x script.sh` for debugging (shows commands as executed).

---

## 🧮 Arrays
```bash
# Define array
fruits=(apple banana cherry)

# Access elements
echo ${fruits[0]}     # apple
echo ${fruits[@]}     # all elements
echo ${#fruits[@]}    # number of elements

# Add new element
fruits+=(mango)

# Loop through array
for fruit in "${fruits[@]}"; do
  echo "$fruit"
done
```

**Notes:**
- Arrays start at index 0.
- Use `${array[@]}` to access all items safely.

---

## 🧩 Case Statements
```bash
read -p "Enter option (start/stop/restart): " action

case $action in
  start)
    echo "Starting service..."
    ;;
  stop)
    echo "Stopping service..."
    ;;
  restart)
    echo "Restarting service..."
    ;;
  *)
    echo "Invalid option."
    ;;
esac
```

**Notes:**
- Acts like a switch-case in other languages.
- `;;` ends each block, `*)` is the default case.

---

## ⚠️ Error Handling
```bash
# Exit immediately if a command fails
set -e

# Custom error message function
error_exit() {
  echo "Error on line $1"
  exit 1
}

trap 'error_exit $LINENO' ERR   # Trap any error and call function

# Example command that may fail
cp missingfile.txt /tmp/
echo "If you see this, no error occurred."
```

**Notes:**
- `set -e` stops execution on first failure.
- `trap` executes a command or function on error or interrupt.
- `$LINENO` returns the current line number.

---

## 🐞 Debugging Techniques
```bash
set -x   # Enable debug mode (prints commands)
set +x   # Disable debug mode

# Run script with debug
bash -x script.sh

# Redirect debug output
bash -x script.sh 2> debug.log
```

**Tips:**
- Use `set -x` at specific places to trace execution.
- Combine `set -euo pipefail` for strict mode (exit on error, unset var, or failed pipe).

---

✅ **Pro Tips:**
- Use `set -e` to exit script on errors.
- Use `set -u` to treat unset variables as errors.
- Use `set -x` for debugging mode.
- Always test scripts in a safe environment before running as root.

