# ZPR — ZYXos Package Repository

> Official package repository for ZYXos — an embedded OS for ESP32-WROVER built from scratch.

---

## What is ZPR?

ZPR (ZYXos Package Repository) is the official repository of packages for [ZYXos](https://github.com/tymek/zyxos). It works similarly to `apt` on Linux or `pkg` on BSD — you can browse, install, and remove programs directly from the ZYXos shell over WiFi, without ever reflashing your ESP32.

Packages are simple `.zyx` scripts — plain text files with ZYXos shell commands executed line by line. No compilation, no binary uploads. Just connect to WiFi, run `zpr update`, and start installing.

---

## Requirements

- ESP32-WROVER (or compatible) with ZYXos firmware flashed
- WiFi connection (use `connect <ssid> <password>` in ZYXos shell)
- ZYXos v1.0.0 or newer

---

## Quick Start

```
# 1. Connect to WiFi
connect MyNetwork MyPassword

# 2. Update package list from ZPR
zpr update

# 3. Browse available packages
zpr list

# 4. Install a package
zpr install snake

# 5. Run it
run snake
```

---

## ZPR Commands

| Command | Description |
|---|---|
| `zpr update` | Fetch latest package index from ZPR |
| `zpr list` | Show all available packages |
| `zpr install <name>` | Download and install a package |
| `zpr remove <name>` | Remove an installed package |
| `zpr info <name>` | Show details about a package |
| `zpr installed` | List locally installed packages |
| `run <name>` | Execute an installed package |

---

## How Packages Work

A ZYXos package is a `.zyx` file — a plain text script where each line is a valid ZYXos shell command:

```bash
# hello.zyx
echo Hello from ZPR!
blink 3 200
calc 2 + 2
echo Done.
```

When you run `zpr install hello`, ZYXos:
1. Downloads `hello.zyx` from this repository via HTTP
2. Saves it to the internal filesystem
3. Makes it available via `run hello`

No reflashing required. New packages become available immediately after `zpr update`.

---

## Repository Structure

```
zpr/
├── index.json          # Package index (fetched by zpr update)
└── packages/
    ├── hello.zyx       # Hello World
    ├── snake.zyx       # Snake game
    ├── sysinfo.zyx     # Extended system info
    └── ...
```

### index.json format

```json
{
  "repo": "ZPR",
  "version": "1.0",
  "updated": "2026-05-31",
  "packages": [
    {
      "name": "hello",
      "description": "Hello World example",
      "version": "1.0",
      "author": "tymek",
      "file": "packages/hello.zyx"
    }
  ]
}
```

---

## Writing a Package

Packages are `.zyx` scripts using standard ZYXos shell commands.

### Available commands in scripts

**Output:**
```
echo <text>         print text
banner <text>       large text banner
morse <text>        convert to Morse code
```

**Files:**
```
cat <file>          read file
nano <file>         edit file
touch <file>        create file
rm <file>           delete file
```

**Hardware:**
```
blink <n> <ms>      blink onboard LED
led on/off/blink    control LED
gpio set <pin> <0/1> set GPIO pin
adc                 read ADC channels
pwm <pin> <f> <d>   PWM output
```

**Network:**
```
ping <host>         ping a host
wget <url>          fetch URL
dns <host>          resolve DNS
```

**Tools:**
```
calc <a> <op> <b>   calculator
timer <sec>         countdown timer
sha256 <text>       SHA-256 hash
base64 <text>       Base64 encode
hex <text>          hex dump
```

**System:**
```
fetch               system info
temp                CPU temperature
meminfo             memory usage
uptime              system uptime
sleep <ms>          wait milliseconds
```

### Example package

```bash
# sysinfo.zyx — Extended system info script
echo === ZYXos System Report ===
fetch
echo --- Memory ---
meminfo
echo --- Temperature ---
temp
echo --- Uptime ---
uptime
echo === End of Report ===
```

---

## Contributing a Package

1. **Fork** this repository
2. Create your `.zyx` script in the `packages/` directory
3. Add an entry to `index.json`:
```json
{
  "name": "yourpackage",
  "description": "What it does",
  "version": "1.0",
  "author": "your-github-username",
  "file": "packages/yourpackage.zyx"
}
```
4. Open a **Pull Request** with a short description

### Package guidelines

- Keep scripts simple and focused on one task
- Use `echo` to show progress and feedback to the user
- Test your script manually in ZYXos shell before submitting
- Package names must be lowercase, no spaces, no special characters
- Include a comment at the top of your script explaining what it does

---

## License

MIT License — free to use, modify and distribute.

---

## Links

- [ZYXos Firmware](https://github.com/tymek/zyxos)
- [ZYXos Documentation](https://github.com/tymek/zyxos/wiki)
- [Report an issue](https://github.com/tymek/zpr/issues)
