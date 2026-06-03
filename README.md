# ZPR — ZYXos Package Repository

> The official package repository for [ZYXos](https://github.com/Tymek-pl/ZYXos) — an embedded OS for ESP32-WROVER.

---

## What is ZPR?

ZPR is the package manager and repository for ZYXos. It works like `apt` on Linux — you connect to WiFi, run `zpr install <package>`, and the program is downloaded and ready to run instantly without reflashing the firmware.

Packages are plain `.zyx` scripts — one ZYXos shell command per line. No compilation needed.

---

## Usage

On your ESP32 running ZYXos:

```
root@zyxos:/$ connect MyNetwork MyPass
root@zyxos:/$ zpr update
root@zyxos:/$ zpr list
root@zyxos:/$ zpr install hello
root@zyxos:/$ run hello
root@zyxos:/$ zpr remove hello
```

---



## Repository Structure

```
zpr/
├── index.json          # package index
└── packages/
    ├── hello.zyx
    ├── sysreport.zyx
    ├── blink_demo.zyx
    ├── netcheck.zyx
    └── morse_demo.zyx
```

---

## Writing a Package

A `.zyx` file is a plain text script with one ZYXos command per line:

```bash
# my_package.zyx
banner HELLO
echo Welcome from my package!
uptime
blink 3 200
echo Done!
```

Supported commands in scripts: all ZYXos shell commands plus:
- `set <var> <value>` — set variable
- `get <var>` — print variable value
- `input <var> <prompt>` — read user input
- `if <a> <op> <b>` / `endif` — condition (op: eq ne gt lt ge le)
- `while <a> <op> <b>` / `endwhile` — loop
- `sleep <ms>` — delay in milliseconds
- `print <text>` — print without newline issues
- `# comment` — comments

### Example with variables

```bash
# counter.zyx
banner COUNTER
set i 0
while $i lt 5
  echo Count: $i
  set i ...
  sleep 500
endwhile
echo Done!
```

---

## Contributing a Package

1. Fork this repository
2. Create your `.zyx` file in `packages/`
3. Add an entry to `index.json`
4. Open a Pull Request

### index.json format

```json
{
  "name": "my_package",
  "description": "Short description",
  "version": "1.0",
  "author": "your-username",
  "file": "my_package.zyx"
}
```

---

## Links

- [ZYXos](https://github.com/Tymek-pl/ZYXos)
- [Report an issue](https://github.com/Tymek-pl/zpr/issues)

---

## License

MIT
