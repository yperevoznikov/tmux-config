# tmux-config

A customized tmux configuration with vim-tmux integration, smart pane switching, the Dracula theme, and a custom tmux-powerline status bar.

Config path: `~/.config/tmux/`

## Repository structure

```
~/.config/tmux/
├── tmux.conf                 # Main tmux configuration
├── tmux-powerline/           # Local tmux-powerline overlay (not committed as a plugin)
│   ├── config.sh             # Powerline segment and theme settings
│   └── themes/
│       └── my.sh             # Custom Dracula-themed powerline theme
└── plugins/                  # TPM plugin installs (gitignored, created on first use)
    └── .keep
```

Plugins are managed by [tpm](https://github.com/tmux-plugins/tpm) and installed into `plugins/` at runtime:

- `tpm` — plugin manager
- `vim-tmux-navigator` — vim-aware pane navigation
- `dracula/tmux` — Dracula color theme
- `erikw/tmux-powerline` — status bar segments

## Features

- **Vim Integration**: Seamless navigation between vim and tmux panes using Ctrl+hjkl (via [vim-tmux-navigator](https://github.com/christoomey/vim-tmux-navigator))
- **Smart Pane Switching**: Intelligent pane navigation that respects vim splits
- **Dracula Theme**: Color scheme via [dracula/tmux](https://github.com/dracula/tmux)
- **Custom Status Bar**: [tmux-powerline](https://github.com/erikw/tmux-powerline) with a local `my` theme and config overlay
- **Mouse Support**: Full mouse support for pane selection and window management
- **Custom Bindings**:
  - `Ctrl+Space` as prefix (instead of default `Ctrl+b`)
  - `Ctrl+Shift+Left/Right` to move and switch windows
  - `Alt+o` to switch to last window
  - New panes and windows inherit current directory
- **Plugin Manager**: Uses [tpm](https://github.com/tmux-plugins/tpm) with plugins under `~/.config/tmux/plugins`

## Installation

### Prerequisites

- tmux 2.9+ (3.1+ recommended for XDG config discovery at `~/.config/tmux/tmux.conf`)
- curl or git
- Git (for cloning this repo and tpm)

### Quick install

Clone the repository into the XDG config directory, then install plugins:

```bash
# Backup existing config if present
if [ -d ~/.config/tmux ]; then
    BACKUP_DIR="$HOME/.config/tmux.back.$(date +%Y%m%d_%H%M%S)"
    mv ~/.config/tmux "$BACKUP_DIR"
    echo "✓ Backed up existing config to: $BACKUP_DIR"
fi

# Clone configuration
git clone https://github.com/yperevoznikov/tmux-config.git ~/.config/tmux
echo "✓ Cloned tmux configuration to ~/.config/tmux"

# Create plugins directory
mkdir -p ~/.config/tmux/plugins

# Clone tpm if not already installed
if [ ! -d ~/.config/tmux/plugins/tpm ]; then
    git clone https://github.com/tmux-plugins/tpm ~/.config/tmux/plugins/tpm
    echo "✓ Installed tmux plugin manager"
fi

echo ""
echo "Installation complete! Start a new tmux session and press Ctrl+Space + I to install plugins."
```

If your tmux version does not read `~/.config/tmux/tmux.conf` automatically, start tmux with an explicit config file:

```bash
tmux -f ~/.config/tmux/tmux.conf
```

Or symlink it to the legacy location:

```bash
ln -sf ~/.config/tmux/tmux.conf ~/.tmux.conf
```

### Manual installation

```bash
# 1. Backup your current config
if [ -d ~/.config/tmux ]; then
    mv ~/.config/tmux ~/.config/tmux.back.$(date +%Y%m%d_%H%M%S)
fi

# 2. Clone the repository
git clone https://github.com/yperevoznikov/tmux-config.git ~/.config/tmux

# 3. Install tpm
git clone https://github.com/tmux-plugins/tpm ~/.config/tmux/plugins/tpm

# 4. Reload tmux
tmux source-file ~/.config/tmux/tmux.conf
```

## Post-installation

After installation, install tpm-managed plugins the first time you start tmux:

```bash
tmux new-session -d
# Press: Ctrl+Space + I (capital i) to install plugins
```

Or run directly:

```bash
~/.config/tmux/plugins/tpm/bin/install_plugins
```

## Configuration

### Key bindings

| Binding | Action |
|---------|--------|
| `Ctrl+Space` | Prefix key (instead of `Ctrl+b`) |
| `Ctrl+h/j/k/l` | Navigate panes (vim-aware) |
| `Ctrl+\` | Navigate to last active pane |
| `Ctrl+Shift+Left` | Move current window left |
| `Ctrl+Shift+Right` | Move current window right |
| `Ctrl+Space + c` | New window in current directory |
| `Ctrl+Space + "` | Split horizontally in current directory |
| `Ctrl+Space + %` | Split vertically in current directory |
| `Ctrl+Space + o` | Switch to last active window |
| `Ctrl+Space + r` | Reload configuration |

### Customization

**General settings and plugins** — edit `~/.config/tmux/tmux.conf`:

- **Location**: Change `New York` in the Dracula weather setting:
  ```bash
  set -g @dracula-fixed-location "New York"
  ```

- **Plugins**: Modify the plugins list:
  ```bash
  set -g @plugin 'tmux-plugins/tpm'
  set -g @plugin 'christoomey/vim-tmux-navigator'
  set -g @plugin 'dracula/tmux'
  set -g @plugin 'erikw/tmux-powerline'
  ```

- **Theme settings**: Adjust Dracula theme options (all prefixed with `@dracula-`)

**Status bar** — edit files under `~/.config/tmux/tmux-powerline/`:

- `config.sh` — segment options, refresh interval, and theme name (`my`)
- `themes/my.sh` — segment layout and Dracula color palette for the powerline status bar

After making changes, reload with: `Ctrl+Space + r`

## Troubleshooting

### Plugins not loading

Make sure tpm is installed:

```bash
git clone https://github.com/tmux-plugins/tpm ~/.config/tmux/plugins/tpm
```

Then press `Ctrl+Space + I` in a tmux session to install plugins.

### Config not picked up

Confirm tmux is loading the XDG config path, or pass the file explicitly:

```bash
tmux -f ~/.config/tmux/tmux.conf
```

### Vim keybindings not working

Ensure vim-tmux-navigator is installed and your vim has the corresponding plugin configured. See [vim-tmux-navigator](https://github.com/christoomey/vim-tmux-navigator) for setup.

### Color scheme issues

If colors don't look right, ensure your terminal supports 256 colors:

```bash
echo $TERM
# Should output: xterm-256color or similar
```

## Reverting changes

To restore a previous configuration:

```bash
rm -rf ~/.config/tmux
mv ~/.config/tmux.back.YYYYMMDD_HHMMSS ~/.config/tmux
tmux source-file ~/.config/tmux/tmux.conf
```

## Inspiration

This configuration is inspired by:

- [faroit's tmux configuration](https://gist.github.com/faroit/ee545a2cec29f5fcc26edb6fe415cfe0)
- [Practical tmux](https://mutelight.org/practical-tmux)

## License

This configuration is provided as-is. Feel free to fork and customize for your needs.
