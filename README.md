# dotfiles

macOS setup with [AeroSpace](https://github.com/nikitabobko/AeroSpace), [SketchyBar](https://github.com/FelixKratz/SketchyBar), [Ghostty](https://ghostty.org), and [Starship](https://starship.rs).

---

## Requirements

Install everything via Homebrew:

```bash
brew install --cask ghostty
brew install nikitabobko/tap/aerospace
brew install felixkratz/formulae/sketchybar
brew install starship
brew install jq          # weather widget popup
brew install nowplaying-cli  # media widget
```

Fonts:

```bash
brew install --cask font-jetbrains-mono
```

SketchyBar also needs two extras:

- **sketchybar-app-font** — for app icons in workspace chips. Follow the install instructions at [github.com/kvndrsslr/sketchybar-app-font](https://github.com/kvndrsslr/sketchybar-app-font).
- **sketchybar-lua** — the config is Lua-based. Follow the install instructions at [github.com/FelixKratz/sketchybar-lua](https://github.com/FelixKratz/sketchybar-lua). The `.so` module is expected at `~/.local/share/sketchybar_lua/`.

---

## macOS settings

### Hide the system menu bar

The system menu bar must be hidden so SketchyBar can replace it without overlap.

`System Settings → Control Center → Menu Bar → Automatically hide and show the menu bar → Always`

### Hide the Dock

`System Settings → Desktop & Dock → Automatically hide and show the Dock → On`

Optionally set the dock show delay to something very long so it never gets in the way:

```bash
defaults write com.apple.dock autohide-delay -float 10 && killall Dock
```

### Grant AeroSpace accessibility access

AeroSpace needs accessibility permissions to move and focus windows.

`System Settings → Privacy & Security → Accessibility → AeroSpace → On`

### Grant AeroSpace screen recording access (optional)

Required if you use the workspace window list feature.

`System Settings → Privacy & Security → Screen & System Audio Recording → AeroSpace → On`

---

## Installation

Clone the repo and symlink (or copy) each config to its expected location:

```bash
git clone <your-repo-url> ~/dotfiles
cd ~/dotfiles

# Ghostty
cp ghostty/config "$HOME/Library/Application Support/com.mitchellh.ghostty/config.ghostty"

# AeroSpace
mkdir -p ~/.config/aerospace
cp aerospace/aerospace.toml ~/.config/aerospace/aerospace.toml

# SketchyBar
mkdir -p ~/.config/sketchybar
cp -r sketchybar/ ~/.config/sketchybar/

# Starship
cp starship.toml ~/.config/starship.toml
```

Make the git scan script executable:

```bash
chmod +x ~/.config/sketchybar/helpers/git_toolkit/git_scan.sh
```

Build the SketchyBar helper binaries (needed for CPU and network widgets):

```bash
cd ~/.config/sketchybar/helpers && make
```

Then restart SketchyBar:

```bash
brew services restart sketchybar
```

---

## Personalisation

**Weather widget** — open `sketchybar/items/widgets/weather.lua` and replace `YourCity` with your city name:

```lua
-- two occurrences, one for the chip and one for the popup
sbar.exec([[curl -s 'https://wttr.in/YourCity?format=%t+%C' ...
```

**Git widget** — open `sketchybar/helpers/git_toolkit/git_scan.sh` and set your projects directory:

```bash
PROJECTS_DIR="${PROJECTS_DIR:-$HOME/Projects}"  # change Projects to your folder
```

Or just export the variable in your shell config and leave the file untouched:

```bash
export PROJECTS_DIR="$HOME/my-projects"
```

---

## AeroSpace keybindings

The modifier for all bindings is `cmd+ctrl`.

### Focus

| Key | Action |
|-----|--------|
| `cmd+ctrl+h` | Focus window left |
| `cmd+ctrl+j` | Focus window down |
| `cmd+ctrl+k` | Focus window up |
| `cmd+ctrl+l` | Focus window right |

### Move window

| Key | Action |
|-----|--------|
| `cmd+ctrl+shift+h` | Move window left |
| `cmd+ctrl+shift+j` | Move window down |
| `cmd+ctrl+shift+k` | Move window up |
| `cmd+ctrl+shift+l` | Move window right |

### Workspaces

| Key | Action |
|-----|--------|
| `cmd+ctrl+1–9` | Switch to workspace 1–9 |
| `cmd+ctrl+shift+1–9` | Move focused window to workspace 1–9 |

### Layout

| Key | Action |
|-----|--------|
| `cmd+ctrl+shift+f` | Toggle fullscreen |
| `cmd+ctrl+shift+t` | Toggle float / tile |
| `cmd+ctrl+shift+r` | Rotate layout (horizontal ↔ vertical) |
| `cmd+ctrl+shift+enter` | Balance window sizes |

### Resize mode

Press `cmd+ctrl+r` to enter resize mode, then:

| Key | Action |
|-----|--------|
| `h` | Decrease width |
| `l` | Increase width |
| `k` | Decrease height |
| `j` | Increase height |
| `enter` / `esc` | Exit resize mode |

### Other

| Key | Action |
|-----|--------|
| `cmd+enter` | Open a new Ghostty window |
