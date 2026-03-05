# 🖥️ SUPERCOMPUTER

> A mesmerizing LED array simulator that creates beautiful chaos through gradual desynchronization.

Watch as perfectly synchronized LEDs slowly drift apart, creating organic, unpredictable light patterns in your terminal.

---

## ✨ Features

- 📐 **Dynamic Auto-Sizing** - Grid automatically adjusts to fill your terminal window
- 🎭 **Stealth Mode** - Pure LED display with zero UI clutter
- 🔒 **Lock Mode** - Fix grid size independent of window dimensions
- 🌈 **Color Modes** - Cycle, gradient, and rainbow color effects
- ⚡ **Real-Time Controls** - Adjust flicker rate, variance, and drift on the fly
- 🎨 **Slightly Customizable** - Modify LED characters and colors
- 🧵 **Some Performance** - Runs smoothly with grids up to 40×40 LEDs
- 💻 **Cross-Platform** - Works on Linux, macOS, and WSL

---

## 🎬 Demo

```
 ● ○ ● ○ ● ○ ● ○ ● ○ ● ○ 
 ○ ● ○ ● ○ ● ● ○ ● ○ ● ○ 
 ● ○ ● ● ○ ● ○ ● ○ ● ○ ● 
 ○ ● ○ ● ● ○ ● ○ ● ● ○ ● 
 ● ○ ● ○ ● ○ ● ○ ● ○ ● ○ 
 ○ ● ● ○ ● ○ ○ ● ○ ● ○ ● 
 ● ○ ● ○ ● ○ ● ○ ● ○ ● ○ 
 ○ ● ○ ● ○ ● ○ ● ● ○ ● ○ 
 ● ○ ● ○ ● ○ ● ○ ● ○ ● ○ 
 ○ ● ○ ● ○ ● ○ ● ○ ● ○ ● 

Status: RUNNING | Base Rate: 2000ms | Variance: 30% | Drift: 20%
```

*LEDs start synchronized, then gradually drift into mesmerizing chaos.*

---

## 🚀 Quick Start

### Installation

```bash
# Clone the repository
git clone https://github.com/IdyllBeast/supercomputer.git
cd supercomputer

# Make executable
chmod +x supercomputer

# Install to local bin (recommended)
mkdir -p ~/.local/bin
cp supercomputer ~/.local/bin/
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

## 📖 Usage

### Command-Line Options

```bash
supercomputer [OPTIONS]

Options:
  -s, --size N          Grid size (3-40, default: 12)
  -r, --rate MS         Flicker rate in milliseconds (50-5000, default: 2000)
  -v, --variance %      Timing variance percentage (0-100, default: 0)
  -d, --drift %         Drift accumulation rate (0-100, default: 1)
  
  --auto                Auto-size grid to terminal dimensions
  --stealth             Hide all UI elements (LEDs only)
  --lock                Prevent grid resize on window change
  --color MODE          Color mode: cycle, gradient, or rainbow
  --simple              Auto-start without interactive controls
  --duration N          Run for N seconds then exit
  
  -h, --help            Show help message
```

### Interactive Controls

When running in interactive mode (default):

| Key | Action |
|-----|--------|
| `s` | Start the simulation |
| `p` | Pause |
| `r` | Reset (resynchronize all LEDs) |
| `+` | Increase flicker rate |
| `-` | Decrease flicker rate |
| `v` | Increase variance |
| `d` | Increase drift |
| `q` | Quit |

---

## 💡 Examples

### Screensaver Mode
```bash
supercomputer --auto --stealth --simple
```
Auto-sizes to fill window, clean display, starts automatically.

### Color Modes
```bash
# LEDs change color each time they flash
supercomputer --color cycle

# Horizontal gradient across the grid
supercomputer --color gradient

# Diagonal rainbow pattern
supercomputer --color rainbow

# Combine with other modes
supercomputer --auto --stealth --color gradient
```

### Maximum Chaos
```bash
supercomputer --auto --stealth -v 100 -d 100 -r 100
```
Fast, unpredictable, mesmerizing.

### Psychedelic Colors
```bash
supercomputer --color cycle -v 80 -d 70 -r 200
```
Rapid color changes with high chaos.

### Slow & Hypnotic
```bash
supercomputer --auto --stealth -v 10 -d 5 -r 1500
```
Gentle, meditative drifting.

### Rainbow Meditation
```bash
supercomputer --auto --stealth --color rainbow -v 0 -d 1 -r 3000
```
Slow, peaceful rainbow pattern.

### Perfect Sync → Gradual Drift
```bash
supercomputer --auto --stealth -v 0 -d 50
```
Starts perfectly synchronized, then slowly drifts apart.

### Recording Mode (Fixed Size)
```bash
supercomputer --stealth --lock -s 16
```
Consistent 16×16 grid that won't change size.

### Colorful Recording
```bash
supercomputer --stealth --lock --color gradient -s 20
```
Fixed size with beautiful gradient colors.

### Dual Monitor Ambient Display
```bash
supercomputer --auto --lock --stealth --simple
```
Calculates size once, locks it, runs forever.

---

## 🎨 Customization

SUPERCOMPUTER is designed to be easily customizable. Edit the `supercomputer` script to:

### Use Built-in Color Modes

```bash
# No editing needed - use command-line flags!
supercomputer --color cycle      # LEDs change color each flash
supercomputer --color gradient   # Left-to-right gradient
supercomputer --color rainbow    # Diagonal rainbow pattern
```

### Change LED Appearance

```python
# In render_grid() method
if led_on:
    line += " ● "  # Change to: █ ★ • X or any character
else:
    line += " ○ "  # Change to: ░ ☆ - or blank spaces
```

### Customize Color Palette

```python
# In get_color() method (around line 256)
COLORS = [
    '\033[91m',  # Red
    '\033[93m',  # Yellow
    '\033[92m',  # Green
    '\033[96m',  # Cyan
    '\033[94m',  # Blue
    '\033[95m',  # Magenta
]
# Change these to any ANSI color codes!
```

### Add Colors Manually

```python
# ANSI color codes
if led_on:
    line += "\033[92m ● \033[0m"  # Green
else:
    line += "\033[91m ○ \033[0m"  # Red
```

---

## 🏗️ Architecture

SUPERCOMPUTER uses an efficient **event-based architecture**:

- Each LED tracks its `next_toggle_time`
- Single update loop checks all LEDs each frame
- O(n) complexity per frame for n LEDs
- ~0.5-1% CPU usage for typical grids

This is much more efficient than the naive approach of one thread per LED.

### Key Components

```
LEDArraySimulator
├─ LED[] - Array of LED objects
├─ update() - Check and toggle LEDs
├─ render_header() - Draw header
├─ render_grid() - Draw LED array
└─ render_footer() - Draw status

LED
├─ next_toggle_time - When to toggle next
├─ is_on - Current state
├─ accumulated_drift - Timing drift
└─ schedule_next_toggle() - Calculate next time
```

---

## 🔧 Requirements

- **Python 3.6+** (usually pre-installed)
- **Terminal with ANSI support** (most modern terminals)
- **Unix-like system** (Linux, macOS, WSL)

### Optional Dependencies

None! Uses only Python standard library:
- `time` - For timing
- `random` - For variance and drift
- `argparse` - For CLI
- `signal` - For window resize handling
- `sys`, `os` - For terminal control

---

### Development

```bash
# Clone the repo
git clone https://github.com/IdyllBeast/supercomputer.git
cd supercomputer

# Make your changes
nano supercomputer

# Test your changes
./supercomputer --simple --duration 5

# Check syntax
python3 -c "import ast; ast.parse(open('supercomputer').read())"
```

---

## 🐛 Troubleshooting

### LEDs not updating?
- Ensure Python 3.6+ is installed: `python3 --version`
- Check file permissions: `chmod +x supercomputer`

### Colors not showing?
- Test terminal color support: `echo -e "\033[91mRed\033[0m"`
- Try a different terminal (gnome-terminal, konsole, xterm)

### Syntax errors after editing?
```bash
python3 -c "import ast; ast.parse(open('supercomputer').read())"
```

### Window resize not working?
- Ensure `SIGWINCH` signal support (Linux/macOS)
- Check that `--lock` flag is not enabled

More troubleshooting in [GUIDE.md](GUIDE.md#troubleshooting).

---

## 📜 License

MIT License - See [LICENSE](LICENSE) file for details.

Free to use, modify, and distribute. Make it your own!

---

## 🙏 Acknowledgments

- Inspired by the beauty of desynchronization in nature
- Built with love for terminal aesthetics
- Created with assistance from Claude AI

---

## 📬 Contact

- **Issues**: [GitHub Issues](https://github.com/IdyllBeast/supercomputer/issues)
- **Discussions**: [GitHub Discussions](https://github.com/IdyllBeast/supercomputer/discussions)

---

## ⭐ Star History

If you find this project useful, please consider giving it a star!

---

## 🎉 Fun Facts

- Each LED is truly independent with its own timing
- The gradual desynchronization mimics firefly behavior
- Perfect for demonstrating chaos theory
- Great conversation starter for your terminal
- Runs on a potato (low resource usage)

---

**Enjoy your SUPERCOMPUTER!** 🚀✨

```
 ███████╗██╗   ██╗██████╗ ███████╗██████╗  ██████╗ ██████╗ ███╗   ███╗██████╗ ██╗   ██╗████████╗███████╗██████╗ 
██╔════╝██║   ██║██╔══██╗██╔════╝██╔══██╗██╔════╝██╔═══██╗████╗ ████║██╔══██╗██║   ██║╚══██╔══╝██╔════╝██╔══██╗
███████╗██║   ██║██████╔╝█████╗  ██████╔╝██║     ██║   ██║██╔████╔██║██████╔╝██║   ██║   ██║   █████╗  ██████╔╝
╚════██║██║   ██║██╔═══╝ ██╔══╝  ██╔══██╗██║     ██║   ██║██║╚██╔╝██║██╔═══╝ ██║   ██║   ██║   ██╔══╝  ██╔══██╗
███████║╚██████╔╝██║     ███████╗██║  ██║╚██████╗╚██████╔╝██║ ╚═╝ ██║██║     ╚██████╔╝   ██║   ███████╗██║  ██║
╚══════╝ ╚═════╝ ╚═╝     ╚══════╝╚═╝  ╚═╝ ╚═════╝ ╚═════╝ ╚═╝     ╚═╝╚═╝      ╚═════╝    ╚═╝   ╚══════╝╚═╝  ╚═╝
```
