

# SUPERCOMPUTER

A terminal-based LED array simulator that creates emergent, desynchronized light patterns.

Insipred by Big Clive and his analog supercomputers. https://www.youtube.com/watch?v=7f8jgvvJe-Q

---

## 🚀 Quick Start

```bash
git clone https://github.com/IdyllBeast/supercomputer.git
cd supercomputer
chmod +x supercomputer

# Optional install
mkdir -p ~/.local/bin
cp supercomputer ~/.local/bin/
```

---

## Usage

```bash
supercomputer [OPTIONS]
```

### Core Options

| Flag               | Description                                    |
| ------------------ | ---------------------------------------------- |
| `-s, --size N`     | Grid size (3–40, default: 12)                  |
| `--auto`           | Auto-fit to terminal                           |
| `--stealth`        | Hide UI                                        |
| `--lock`           | Disable resize                                 |
| `--ascii`          | Animated character mode                        |
| `--color MODE`     | `cycle`, `gradient`, `rainbow`, `rainbow-wave` |
| `-r, --rate MS`    | Flicker rate (default: 2000)                   |
| `-v, --variance %` | Timing randomness                              |
| `-d, --drift %`    | Desync accumulation                            |
| `--simple`         | Auto-start (no controls)                       |
| `--duration N`     | Exit after N seconds                           |

---

## Controls (Interactive Mode)

| Key     | Action            |
| ------- | ----------------- |
| `s`     | Start             |
| `p`     | Pause             |
| `r`     | Reset             |
| `+ / -` | Adjust rate       |
| `v`     | Increase variance |
| `d`     | Increase drift    |
| `q`     | Quit              |

---

## Examples

```bash
# Auto-sized display
supercomputer --auto

# Clean LED-only mode
supercomputer --stealth

# ASCII + rainbow wave
supercomputer --ascii --color rainbow-wave

# Full ambient mode
supercomputer --auto --stealth --ascii --color rainbow-wave --simple

# High chaos
supercomputer -v 100 -d 100 -r 100
```

---

## Features

* Event-based (no per-LED threads)
* Auto-resizing grid
* Multiple color modes
* ASCII animation system
* Flicker-free rendering (alternate buffer)
* Real-time parameter control
* Cross-platform (Linux/macOS/WSL)

---

## Customization

Edit the script to modify:

* LED characters (`render_grid`)
* Color palette (`get_color_code`)
* ASCII sets (character arrays)

Example:

```python
line += " ● "  # change LED appearance
```

---

## Requirements

* Python 3.6+
* ANSI-compatible terminal

---

## Troubleshooting

* **No colors:** terminal lacks ANSI support
* **Flicker:** use a modern terminal (kitty, alacritty, etc.)
* **ASCII issues:** install Unicode fonts
* **Flags missing:** update your script version

---

License MIT



