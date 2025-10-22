# 🐚 Bash Scripting Cheat Sheet

## 🧠 Basics
```bash
#!/bin/bash    # Shebang: tells the system to use bash
# Comment line

echo "Hello, World!"       # Print text
echo "User: $USER"         # Print variable value
```

---

## 📂 Variables
```bash
name="Amro"
echo "Hello $name"         # Access variable
echo "Hello ${name}"       # Same but safer

age=20
echo "Age: $age"
```
> ⚠️ No spaces around `=`

---

## 📥 User Input
```bash
read name
echo "Welcome $name"

read -p "Enter your age: " age
echo "You are $age years old"
```

---

## ⚙️ Conditionals
```bash
if [ "$USER" == "amro" ]; then
    echo "Hi Amro!"
elif [ "$USER" == "root" ]; then
    echo "Hello root"
else
    echo "Unknown user"
fi
```

### Operators
| Type | Example | Description |
|------|----------|-------------|
| String | `==`, `!=`, `-z`, `-n` | Compare strings |
| Number | `-eq`, `-ne`, `-gt`, `-lt`, `-ge`, `-le` | Numeric comparison |
| File | `-e`, `-f`, `-d`, `-r`, `-w`, `-x` | Check existence, type, permissions |

Example:
```bash
if [ -f "/etc/passwd" ]; then
    echo "File exists"
fi
```

---

## 🔁 Loops

### For Loop
```bash
for i in 1 2 3; do
  echo "Number $i"
done

for i in {1..5}; do
  echo "Count: $i"
done
```

### While Loop
```bash
count=1
while [ $count -le 5 ]; do
  echo $count
  ((count++))
done
```

### Until Loop
```bash
count=1
until [ $count -gt 5 ]; do
  echo $count
  ((count++))
done
```

---

## 🧩 Functions
```bash
greet() {
  echo "Hello, $1!"
}

greet Amro
```

Using return values:
```bash
add() {
  echo $(($1 + $2))
}
result=$(add 5 3)
echo "Result: $result"
```

---

## 📅 Date and Time
```bash
date                    # Current date/time
date +"%Y-%m-%d"        # Format: YYYY-MM-DD
date +"%H:%M:%S"        # Time only
```

---

## 📁 Files & Directories
```bash
mkdir mydir
cd mydir
touch file.txt
rm file.txt
cp file1 file2
mv old new
```

---

## 🧮 Arithmetic
```bash
x=5
y=3
echo $((x + y))
echo $((x * y))
```

Or using `expr`:
```bash
expr $x + $y
```

---

## 🔢 Arrays
```bash
fruits=("apple" "banana" "cherry")
echo ${fruits[0]}
echo ${fruits[@]}
echo "Length: ${#fruits[@]}"
```

Add element:
```bash
fruits+=("orange")
```

---

## 📜 Strings
```bash
text="hello world"
echo ${#text}            # Length
echo ${text:0:5}         # Substring
echo ${text/world/bash}  # Replace
```

---

## ⚡ Command Substitution
```bash
current_dir=$(pwd)
echo "You are in $current_dir"
```

---

## 🧩 Exit Status
```bash
ls /etc/passwd
echo $?      # 0 means success

ls /no/such/file
echo $?      # Non-zero means error
```

---

## 🧱 Case Statement
```bash
read -p "Enter option (a/b): " opt
case $opt in
  a) echo "You chose A";;
  b) echo "You chose B";;
  *) echo "Invalid option";;
esac
```

---

## ⚙️ Useful Shortcuts
| Shortcut | Description |
|-----------|-------------|
| `Ctrl + C` | Stop script |
| `Ctrl + D` | End input |
| `Ctrl + L` | Clear screen |
| `!!` | Repeat last command |
| `!ls` | Repeat last command starting with `ls` |

---

## 🧰 Common Tools
| Command | Purpose |
|----------|----------|
| `grep "word" file` | Search text |
| `sed 's/old/new/g' file` | Replace text |
| `awk '{print $1}' file` | Extract column |
| `cut -d':' -f1 file` | Cut by delimiter |
| `tr 'a-z' 'A-Z'` | Translate characters |

---

✅ **Tip:** Always make your script executable:
```bash
chmod +x script.sh
./script.sh
```
