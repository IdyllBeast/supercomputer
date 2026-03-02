# SUPERCOMPUTER - Installation & Customization Guide

Complete guide for installing and customizing your LED array simulator.

---

## 📦 Installation

### Quick Install (Recommended)

```bash
# Create local bin directory
mkdir -p ~/.local/bin

# Copy the script
cp supercomputer ~/.local/bin/

# Make it executable
chmod +x ~/.local/bin/supercomputer

# Add to PATH (if not already there)
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc

# Reload shell
source ~/.bashrc

# Test it!
supercomputer --help
```

### System-Wide Install (Requires sudo)

```bash
sudo cp supercomputer /usr/local/bin/
sudo chmod +x /usr/local/bin/supercomputer
supercomputer --help
```

### Quick Alias (No Installation)

```bash
# Add to ~/.bashrc or ~/.zshrc
alias supercomputer='/full/path/to/supercomputer'

# Reload
source ~/.bashrc
```

### Verify Installation

```bash
supercomputer --version  # Should work if installed correctly
supercomputer --help     # Show all options
```

---

## 🚀 Quick Start

```bash
# Default settings (12×12 grid, 2 second flicker, minimal drift)
supercomputer

# Auto-size to fill your terminal
supercomputer --auto

# Clean display with no UI
supercomputer --stealth

# The ultimate combo - auto-size + clean display + auto-start
supercomputer --auto --stealth --simple
```

---

## ⚙️ Configuration Options

### Grid Size

```bash
# Fixed size
supercomputer -s 8          # 8×8 grid
supercomputer -s 15         # 15×15 grid
supercomputer -s 20         # 20×20 grid

# Auto-size (calculates from terminal dimensions)
supercomputer --auto

# Auto-size then lock (won't change if window resizes)
supercomputer --auto --lock
```

### Timing & Behavior

```bash
# Rate: How fast LEDs flicker (in milliseconds)
supercomputer -r 500        # Fast (500ms)
supercomputer -r 2000       # Slow (2 seconds) - default
supercomputer -r 5000       # Very slow (5 seconds)

# Variance: Random timing variation (0-100%)
supercomputer -v 0          # Perfect sync - default
supercomputer -v 30         # Moderate chaos
supercomputer -v 100        # Maximum randomness

# Drift: How quickly timing diverges (0-100%)
supercomputer -d 1          # Minimal drift - default
supercomputer -d 50         # High drift
supercomputer -d 100        # Maximum drift
```

### Display Modes

```bash
# Normal mode (default) - with header, borders, status
supercomputer

# Stealth mode - LEDs only, no UI
supercomputer --stealth

# Lock mode - prevent grid resize
supercomputer --lock

# Simple mode - auto-start (no interactive controls)
supercomputer --simple
```

### Combinations

```bash
# Maximum chaos
supercomputer --auto --stealth -v 100 -d 100 -r 100

# Slow hypnotic
supercomputer --auto --stealth -v 10 -d 5 -r 1500

# Perfect sync → gradual drift
supercomputer --auto --stealth -v 0 -d 50

# Screensaver mode
supercomputer --auto --stealth --simple

# Recording mode (fixed size for consistency)
supercomputer --stealth --lock -s 16
```

---

## 🎨 Customization

All customization is done by editing the `supercomputer` file. Open it with:

```bash
nano supercomputer
# or
vim supercomputer
# or
code supercomputer  # VS Code
```

### 1. Change LED Characters

**Location:** `render_grid()` method (around line 245)

**Current:**
```python
if led_on:
    line += " ● "  # Filled circle
else:
    line += " ○ "  # Empty circle
```

**Alternatives:**

```python
# Solid blocks
if led_on:
    line += " █ "
else:
    line += " ░ "

# Stars
if led_on:
    line += " ★ "
else:
    line += " ☆ "

# Simple dots
if led_on:
    line += " • "
else:
    line += "   "  # Blank

# Binary
if led_on:
    line += " 1 "
else:
    line += " 0 "

# Custom characters
if led_on:
    line += " X "
else:
    line += " - "
```

### 2. Add Colors

**Add ANSI color codes:**

```python
# Green when on, red when off
if led_on:
    line += "\033[92m ● \033[0m"  # Bright green
else:
    line += "\033[91m ○ \033[0m"  # Bright red

# Cyan when on, dark gray when off
if led_on:
    line += "\033[96m ● \033[0m"
else:
    line += "\033[90m ○ \033[0m"

# Rainbow effect (random colors)
import random
colors = ['\033[91m', '\033[92m', '\033[93m', '\033[94m', '\033[95m', '\033[96m']
if led_on:
    color = random.choice(colors)
    line += f"{color} ● \033[0m"
else:
    line += "\033[90m ○ \033[0m"
```

**Color Reference:**
- `\033[91m` - Bright Red
- `\033[92m` - Bright Green
- `\033[93m` - Bright Yellow
- `\033[94m` - Bright Blue
- `\033[95m` - Bright Magenta
- `\033[96m` - Bright Cyan
- `\033[90m` - Dark Gray
- `\033[0m` - Reset color

### 3. Change Border Style

**Location:** `render_grid()` method (around line 250)

**Current:**
```python
print("┌" + "─" * (self.grid_size * 4 - 1) + "┐")  # Top
line = "│"  # Sides
print("└" + "─" * (self.grid_size * 4 - 1) + "┘")  # Bottom
```

**Alternatives:**

```python
# Simple ASCII
print("+" + "-" * (self.grid_size * 4 - 1) + "+")
line = "|"
print("+" + "-" * (self.grid_size * 4 - 1) + "+")

# Double lines
print("╔" + "═" * (self.grid_size * 4 - 1) + "╗")
line = "║"
print("╚" + "═" * (self.grid_size * 4 - 1) + "╝")

# Thick borders
print("┏" + "━" * (self.grid_size * 4 - 1) + "┓")
line = "┃"
print("┗" + "━" * (self.grid_size * 4 - 1) + "┛")

# No border (remove these lines entirely)
# Just print the grid without borders
```

### 4. Modify Header

**Location:** `render_header()` method (around line 230)

**Current:**
```python
print("=" * header_width)
print(size_info.center(header_width))
print("=" * header_width)
```

**Alternatives:**

```python
# ASCII art style
print("╔" + "═" * (header_width - 2) + "╗")
print("║" + size_info.center(header_width - 2) + "║")
print("╚" + "═" * (header_width - 2) + "╝")

# Banner style
print("\n" + "▓" * header_width)
print("▓" + size_info.center(header_width - 2) + "▓")
print("▓" * header_width + "\n")

# Minimal (no decorations)
print(size_info)
print()

# Custom title
print("=" * header_width)
print("MY AWESOME LED ARRAY".center(header_width))
print(size_info.center(header_width))
print("=" * header_width)
```

### 5. Add Row Spacing

**Location:** `render_grid()` method, in the loop that prints rows

**Current:**
```python
for row in grid:
    # ... build line ...
    print(line)
```

**With spacing:**
```python
for row in grid:
    # ... build line ...
    print(line)
    print()  # Add blank line between rows
```

**Double spacing:**
```python
for row in grid:
    # ... build line ...
    print(line)
    print()  # First blank line
    print()  # Second blank line
```

### 6. Change Default Settings

**Location:** Constants at the top (around line 16-37)

```python
# Current defaults
DEFAULT_GRID_SIZE = 12
DEFAULT_RATE = 2000      # 2 seconds
DEFAULT_VARIANCE = 0     # No variance
DEFAULT_DRIFT = 1        # Minimal drift

# Examples - change to what you prefer:
DEFAULT_GRID_SIZE = 20   # Larger grid
DEFAULT_RATE = 500       # Faster flicker
DEFAULT_VARIANCE = 30    # More chaos
DEFAULT_DRIFT = 40       # More drift
```

### 7. Adjust Display Speed

**Location:** Constants at the top

```python
# Current
DISPLAY_FPS = 10  # 10 frames per second

# Smoother (uses more CPU)
DISPLAY_FPS = 30  # 30 frames per second

# Slower (uses less CPU)
DISPLAY_FPS = 5   # 5 frames per second
```

### 8. Hide Footer

**Location:** `print_grid()` method (around line 300)

**To hide footer completely:**
```python
def print_grid(self):
    self.clear_screen()
    
    if not self.stealth:
        self.render_header()
    
    self.render_grid()
    
    # Comment out or remove this line:
    # if not self.stealth:
    #     self.render_footer()
```

---

## 🎭 Creating Custom Themes

### Cyberpunk Theme

```python
# In render_grid(), replace LED rendering with:
if led_on:
    line += "\033[96m █ \033[0m"  # Cyan solid block
else:
    line += "\033[90m ░ \033[0m"  # Dark gray shade

# In render_header(), replace with:
print("\033[96m" + "▓" * header_width + "\033[0m")
print("\033[95m" + size_info.center(header_width) + "\033[0m")
print("\033[96m" + "▓" * header_width + "\033[0m")

# Use thick borders
print("\033[95m┏" + "━" * (self.grid_size * 4 - 1) + "┓\033[0m")
```

### Matrix Theme

```python
# Green monochrome
if led_on:
    line += "\033[92m ● \033[0m"  # Bright green
else:
    line += "\033[32m ○ \033[0m"  # Dim green

# Or use actual matrix characters
if led_on:
    import random
    char = random.choice(['0', '1', 'ﾊ', 'ﾐ', 'ﾋ', 'ｰ', 'ｳ', 'ｼ'])
    line += f"\033[92m {char} \033[0m"
else:
    line += "\033[32m   \033[0m"
```

### Minimalist Theme

```python
# In render_grid()
if led_on:
    line += " ● "
else:
    line += "   "  # Completely blank when off

# Remove all borders
# Remove header and footer by using --stealth flag
```

### Retro Terminal Theme

```python
# Amber monochrome
COLOR = "\033[38;5;214m"  # Amber
RESET = "\033[0m"

if led_on:
    line += f"{COLOR} █ {RESET}"
else:
    line += f"{COLOR} ░ {RESET}"

# Use ASCII borders
print("+" + "-" * (self.grid_size * 4 - 1) + "+")
```

---

## 🔧 Advanced Customization

### Make LEDs Blink Instead of Toggle

**Location:** `render_grid()` method

```python
# Current (toggle on/off)
if led_on:
    line += " ● "

# Blinking (only show when on, blank when off)
if led_on:
    line += " ● "
else:
    line += "   "  # Completely blank
```

### Add LED Position Numbers

```python
# In render_grid(), track position
position = 0
for row in grid:
    line = "│"
    for led_on in row:
        if led_on:
            line += f" {position:2d} "
        else:
            line += "    "
        position += 1
    print(line)
```

### Create Patterns (e.g., Checkerboard Start)

**Location:** `start()` method in LEDArraySimulator

```python
def start(self):
    if not self.running:
        self.running = True
        self.start_time = time.monotonic()
        
        # Checkerboard pattern initialization
        for i, led in enumerate(self.leds):
            row = i // self.grid_size
            col = i % self.grid_size
            
            # Alternate on/off in checkerboard
            if (row + col) % 2 == 0:
                led.is_on = True
            else:
                led.is_on = False
            
            led.schedule_next_toggle(self.start_time)
```

---

## 💡 Tips & Tricks

### Performance Tuning

```bash
# For large grids, reduce display FPS
# Edit the constant: DISPLAY_FPS = 5

# For smoother animation on small grids
# Edit the constant: DISPLAY_FPS = 30
```

### Recording Video

```bash
# Use lock mode for consistent size
supercomputer --stealth --lock -s 16

# Use tools like asciinema
asciinema rec supercomputer-demo.cast
supercomputer --auto --stealth --simple
# Press Ctrl+C to stop recording
```

### Screenshot Mode

```bash
# Fixed size, clean display
supercomputer --stealth --lock -s 20 --simple

# Let it run for a bit, then screenshot
```

### Dual Monitor Ambient Display

```bash
# On your second monitor:
supercomputer --auto --lock --stealth --simple -v 40 -d 30 -r 800

# Calculates initial size, locks it, runs forever
```

### Testing Customizations

```bash
# After editing the file:
./supercomputer --simple --duration 10

# Runs for 10 seconds then exits automatically
# Great for quick testing
```

---

## 🆘 Troubleshooting

### Syntax Errors After Editing

```bash
# Check for Python syntax errors
python3 -c "import ast; ast.parse(open('supercomputer').read())"

# If error, look at the line number and fix the syntax
```

### LEDs Not Updating

- Check that you didn't accidentally break the `update()` method
- Make sure `simulator.update()` is being called in the main loop
- Verify constants are defined at the top

### Display Issues

- Make sure your terminal supports ANSI escape codes
- Try a different terminal (xterm, gnome-terminal, konsole, etc.)
- Check that color codes have proper `\033[0m` reset

### Colors Not Working

```bash
# Test if your terminal supports colors
echo -e "\033[91mRed\033[0m \033[92mGreen\033[0m \033[94mBlue\033[0m"

# If colors don't show, your terminal may not support them
```

---

## 📝 Example Customizations

### 1. Larger Spacing

```python
# In render_grid()
for row in grid:
    line = "│"
    for led_on in row:
        if led_on:
            line += "  ●  "  # Extra spaces
        else:
            line += "  ○  "
    line += "│"
    print(line)
    print()  # Row spacing
```

### 2. Show Percentage On

```python
# In render_footer(), add:
on_count = sum(1 for led in self.leds if led.is_on)
total = len(self.leds)
percentage = (on_count / total) * 100
print(f"LEDs On: {on_count}/{total} ({percentage:.1f}%)")
```

### 3. Rainbow LEDs

```python
# In render_grid()
colors = ['\033[91m', '\033[93m', '\033[92m', '\033[96m', '\033[94m', '\033[95m']
position = 0
for row in grid:
    line = "│"
    for led_on in row:
        color = colors[position % len(colors)]
        if led_on:
            line += f"{color} ● \033[0m"
        else:
            line += " ○ "
        position += 1
    line += "│"
    print(line)
```

---

## 🔗 Quick Reference

**Installation:**
```bash
cp supercomputer ~/.local/bin/ && chmod +x ~/.local/bin/supercomputer
```

**Usage:**
```bash
supercomputer [OPTIONS]
```

**Common Combos:**
```bash
supercomputer --auto --stealth --simple     # Ultimate mode
supercomputer -s 20 -v 50 -d 40            # Chaotic 20×20
supercomputer --stealth --lock -s 16        # Recording mode
```

**Customize:**
```bash
nano supercomputer    # Edit LED appearance, colors, borders
```

**Help:**
```bash
supercomputer --help  # Show all options
```

---

Enjoy customizing your SUPERCOMPUTER! 🚀✨
