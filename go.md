# 🦫 Installing Go and Go-Based Tools (like `hakrevdns`) on Linux

## 🧭 Step 1: Check if Go is Installed
```bash
go version
```
If it says `command not found`, continue below.

---

## 🧩 Step 2: Download and Install Go

Visit the official Go download page:
👉 [https://go.dev/dl/](https://go.dev/dl/)

Then download the latest Linux tarball, for example:
```bash
wget https://go.dev/dl/go1.23.2.linux-amd64.tar.gz
```

Remove any old installation:
```bash
sudo rm -rf /usr/local/go
```

Extract it to `/usr/local`:
```bash
sudo tar -C /usr/local -xzf go1.23.2.linux-amd64.tar.gz
```

---

## ⚙️ Step 3: Add Go to Your PATH

If you use **Zsh** (default on Kali, Parrot, etc.):
```bash
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.zshrc
source ~/.zshrc
```

If you use **Bash**:
```bash
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
source ~/.bashrc
```

Check Go version:
```bash
go version
```
✅ Should print something like `go version go1.23.2 linux/amd64`

---

## 🧠 Understanding Go's Folder Structure

It’s important to understand how Go separates **its compiler** from **user-installed tools**:

| Location | Purpose | Owner |
|-----------|----------|--------|
| `/usr/local/go` | Go compiler & standard library | Root/system |
| `~/go/bin` | Where Go installs new tools (like `httpx`, `hakrevdns`, etc.) | Your user |
| `/usr/local/bin` | Where you can move tools to make them globally available | Root/system |

### 🔍 Explanation
- `/usr/local/go` → contains the Go runtime and compiler. You should **not** install tools here.
- `~/go/bin` → Go automatically puts new binaries here when you run `go install ...`.
- `/usr/local/bin` → already in your PATH system-wide, so moving tools here makes them available everywhere.

Even though Go is installed in `/usr/local/go`, **your tools are still built under `~/go/bin` by default.** This design keeps system files separate from user data.

If you want your tools to be globally accessible, you can manually move them:
```bash
sudo mv ~/go/bin/hakrevdns /usr/local/bin/
```

Now you can run it from anywhere:
```bash
hakrevdns -h
```

---

## 🧰 Step 4: Install a Go Tool (Example: `hakrevdns`)

### Method 1 — Regular Install (Default Behavior)
```bash
go install github.com/hakluke/hakrevdns@latest
```
This builds the binary in:
```
~/go/bin/hakrevdns
```
Move it globally:
```bash
sudo mv ~/go/bin/hakrevdns /usr/local/bin/
```
Test:
```bash
hakrevdns -h
```

---

### Method 2 — Automated Helper Function (Optional)
Add this function to your `~/.zshrc`:
```bash
goinstall() {
    go install "$1"
    tool=$(basename "$1" | cut -d'@' -f1)
    sudo mv ~/go/bin/$tool /usr/local/bin/ 2>/dev/null
    echo "$tool installed globally."
}
```
Reload your shell:
```bash
source ~/.zshrc
```
Now install globally in one line:
```bash
goinstall github.com/hakluke/hakrevdns@latest
```
✅ Output example:
```
hakrevdns installed globally.
```

---

## 🧠 Notes
- `/usr/local/go/bin` → Go compiler
- `~/go/bin` → Build output for tools
- `/usr/local/bin` → Global tools location

---

### 🔍 Verify
```bash
which go
which hakrevdns
```
Both should show paths under `/usr/local/`.

---

### ✅ Example Summary
| Task | Command |
|------|----------|
| Install Go | `sudo tar -C /usr/local -xzf go1.23.2.linux-amd64.tar.gz` |
| Add PATH | `export PATH=$PATH:/usr/local/go/bin` |
| Install tool | `go install github.com/hakluke/hakrevdns@latest` |
| Move globally | `sudo mv ~/go/bin/hakrevdns /usr/local/bin/` |

